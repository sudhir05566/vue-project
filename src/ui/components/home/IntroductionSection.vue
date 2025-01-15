<template>
  <section class="introduction">
    <div class="introduction-content">
      <h2>What is WizFit Challenge?</h2>
      <button class="watch-video-btn">
        <span class="play-icon">▶</span> Watch Video
      </button>
    </div>
    <img
      src="../../../../public/player.png"
      alt="WizFit Character"
      class="center-image"
    />
  </section>
</template>

<script>
export default {}
</script>

<style scoped>
.introduction {
  text-align: center;
}

.center-image {
  margin-top: 20px;
  width: 300px;
  max-width: 100%;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
h2 {
  color: hsl(346.89deg 64.26% 46.08%)!important;
  font-size: 25px;
  font-weight: bold;
}

.play-icon {
  font-size: 18px;
  display: inline-block;
}

.introduction-content {
  text-align: center;
  margin: 20px;
  gap: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.watch-video-btn {
  background-color: hsl(346.89deg 64.26% 46.08%);
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s;
}

.watch-video-btn:hover {
  background-color: #e00044;
}

@media (max-width: 600px) {
  .introduction-content {
    margin: 10px;
    gap: 10px;
    display: block;
  }

  .watch-video-btn {
    padding: 8px 16px;
    font-size: 14px;
  }

  .play-icon {
    font-size: 16px;
  }
}
</style>
