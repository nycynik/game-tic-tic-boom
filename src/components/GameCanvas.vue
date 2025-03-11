<script setup>
import { onMounted } from 'vue'
import Phaser from 'phaser'

onMounted(() => {
  let game;

  const config = {
    type: Phaser.AUTO,
    width: 1280,
    height: 720,
    parent: 'game-container',
    scene: { preload, create, update }
  };

  game = new Phaser.Game(config);

  function preload() {
    this.load.image('sky', 'https://labs.phaser.io/assets/skies/space3.png');
    this.load.image('star', 'https://labs.phaser.io/assets/demoscene/star.png');
  }

  function create() {
    this.add.image(640, 360, 'sky'); // Background

    // Draw grid (optional)
    for (let x = 0; x < 1280; x += 40) {
      for (let y = 0; y < 720; y += 40) {
        this.add.rectangle(x + 20, y + 20, 40, 40, 0x222222).setStrokeStyle(1, 0xffffff);
      }
    }

    // Add a piece
    this.piece = this.add.sprite(40 * 5, 40 * 5, 'star');
    this.piece.setOrigin(0.5, 0.5); // Centered

    // WASD Movement
    this.cursors = this.input.keyboard.addKeys({
      up: Phaser.Input.Keyboard.KeyCodes.W,
      down: Phaser.Input.Keyboard.KeyCodes.S,
      left: Phaser.Input.Keyboard.KeyCodes.A,
      right: Phaser.Input.Keyboard.KeyCodes.D
    });

    // Q/E for rotation
    this.rotationKeys = this.input.keyboard.addKeys({
      rotateLeft: Phaser.Input.Keyboard.KeyCodes.Q,
      rotateRight: Phaser.Input.Keyboard.KeyCodes.E
    });
  }

  function update() {
    if (!this.piece) return;

    // Move in 40px steps
    if (Phaser.Input.Keyboard.JustDown(this.cursors.left)) {
      this.piece.x = Math.max(40, this.piece.x - 40);
    }
    if (Phaser.Input.Keyboard.JustDown(this.cursors.right)) {
      this.piece.x = Math.min(1280 - 40, this.piece.x + 40);
    }
    if (Phaser.Input.Keyboard.JustDown(this.cursors.up)) {
      this.piece.y = Math.max(40, this.piece.y - 40);
    }
    if (Phaser.Input.Keyboard.JustDown(this.cursors.down)) {
      this.piece.y = Math.min(720 - 40, this.piece.y + 40);
    }

    // Rotate with Q/E
    if (Phaser.Input.Keyboard.JustDown(this.rotationKeys.rotateLeft)) {
      this.piece.angle -= 90;
    }
    if (Phaser.Input.Keyboard.JustDown(this.rotationKeys.rotateRight)) {
      this.piece.angle += 90;
    }
  }
});
</script>

<template>
  <div class="flex justify-center items-center w-full h-screen bg-gray-900">
    <div id="game-container"></div>
  </div>
</template>

<style scoped>
#game-container {
  width: 1280px;
  height: 720px;
}
</style>
