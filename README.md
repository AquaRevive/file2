<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Polvo 3D - Distribuição do Projeto</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: transparent; /* Fundo transparente para o vídeo */
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r146/three.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.10.4/gsap.min.js"></script>
  <script>
    // Configurações iniciais
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Luzes
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(5, 5, 5).normalize();
    scene.add(light);

    const ambientLight = new THREE.AmbientLight(0x404040);
    scene.add(ambientLight);

    // Criação do polvo
    const bodyGeometry = new THREE.SphereGeometry(1, 32, 32);
    const bodyMaterial = new THREE.MeshPhongMaterial({ color: 0x00aaff, shininess: 100 });
    const body = new THREE.Mesh(bodyGeometry, bodyMaterial);
    scene.add(body);

    // Tentáculos (aspectos do projeto)
    const tentacles = [];
    const percentages = [40, 30, 20, 10]; // Distribuição de porcentagens
    const colors = [0xff0000, 0x00ff00, 0x0000ff, 0xffff00]; // Cores para cada tentáculo

    for (let i = 0; i < 4; i++) {
      const tentacleGeometry = new THREE.CylinderGeometry(0.1, 0.1, 2, 32);
      const tentacleMaterial = new THREE.MeshPhongMaterial({ color: colors[i] });
      const tentacle = new THREE.Mesh(tentacleGeometry, tentacleMaterial);

      // Posiciona os tentáculos ao redor do corpo
      const angle = (i / 4) * Math.PI * 2;
      tentacle.position.x = Math.cos(angle) * 1.5;
      tentacle.position.y = Math.sin(angle) * 1.5;
      tentacle.position.z = 0;

      // Rotaciona os tentáculos
      tentacle.rotation.z = angle + Math.PI / 2;

      scene.add(tentacle);
      tentacles.push(tentacle);
    }

    // Posiciona a câmera
    camera.position.z = 5;

    // Animação dos tentáculos
    const animateTentacles = () => {
      tentacles.forEach((tentacle, i) => {
        gsap.to(tentacle.scale, {
          y: percentages[i] / 20, // Ajusta o tamanho com base na porcentagem
          duration: 2,
          ease: "power2.out",
        });
      });
    };

    // Inicia a animação
    animateTentacles();

    // Renderização da cena
    const animate = () => {
      requestAnimationFrame(animate);

      // Rotaciona o polvo
      body.rotation.x += 0.01;
      body.rotation.y += 0.01;

      renderer.render(scene, camera);
    };

    animate();

    // Ajusta o tamanho da cena ao redimensionar a janela
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  </script>
</body>
</html>
