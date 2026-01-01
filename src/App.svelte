<script lang="ts">
  import { onMount } from 'svelte';

  let effectRunning = false;

  onMount(() => {
    const path = window.location.pathname;
    const segments = path.split("/").filter(Boolean);
    if (segments.length > 0) {
      const newUrl = window.location.origin + "/" + window.location.search + window.location.hash;
      window.location.replace(newUrl);
      return;
    }
  });

  function playBubbleEffect(): void {
    if (effectRunning) return;
    effectRunning = true;
    const bubbleSound = document.getElementById("bubble-sound") as HTMLAudioElement | null;
    if (bubbleSound) {
      bubbleSound.currentTime = 0;
      bubbleSound.play().catch(() => {});
    }
    spawnBubblesFromBottom();
    setTimeout(() => {
      effectRunning = false;
    }, 5000);
  }

  function spawnBubblesFromBottom(): void {
    const layer = document.getElementById("bubble-layer");
    const width = window.innerWidth;
    const bubbleCount = 150;

    for (let i = 0; i < bubbleCount; i++) {
      const bubble = document.createElement("div");
      bubble.className = "bubble";
      const size = 10 + Math.random() * 20;
      const leftPercent = Math.random() * 100;
      const delay = Math.random() * 1.0;
      const duration = 3.2 + Math.random() * 2.0;

      const colors = [
        "rgba(239, 68, 68, 0.45)",
        "rgba(249, 115, 22, 0.45)",
        "rgba(234, 179, 8, 0.45)",
        "rgba(34, 197, 94, 0.45)",
        "rgba(59, 130, 246, 0.45)",
        "rgba(139, 92, 246, 0.45)",
        "rgba(236, 72, 153, 0.45)"
      ];
      const color = colors[Math.floor(Math.random() * colors.length)];
      bubble.style.width = `${size}px`;
      bubble.style.height = `${size}px`;
      bubble.style.left = `${(leftPercent / 100) * width}px`;
      bubble.style.background = color;
      bubble.style.animationDuration = `${duration}s`;
      bubble.style.animationDelay = `${delay}s`;

      layer?.appendChild(bubble);

      setTimeout(() => {
        bubble.remove();
      }, (duration + delay) * 1000 + 300);
    }
  }
</script>

<main class="page">
  <section class="card">
    <h1 class="title">We&rsquo;re currently offline</h1>
    <p class="subtitle">
      Our site is temporarily unavailable while we perform maintenance.
      Thank you for your patience.
    </p>
    <div class="gear-icon" aria-label="Maintenance indicator">
      <div class="gear-wrap">
        <svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" fill="currentColor" class="bi bi-gear" viewBox="0 0 16 16">
          <path d="M8 4.754a3.246 3.246 0 1 0 0 6.492 3.246 3.246 0 0 0 0-6.492M5.754 8a2.246 2.246 0 1 1 4.492 0 2.246 2.246 0 0 1-4.492 0"/>
          <path d="M9.796 1.343c-.527-1.79-3.065-1.79-3.592 0l-.094.319a.873.873 0 0 1-1.255.52l-.292-.16c-1.64-.892-3.433.902-2.54 2.541l.159.292a.873.873 0 0 1-.52 1.255l-.319.094c-1.79.527-1.79 3.065 0 3.592l.319.094a.873.873 0 0 1 .52 1.255l-.16.292c-.892 1.64.901 3.434 2.541 2.54l.292-.159a.873.873 0 0 1 1.255.52l.094.319c.527 1.79 3.065 1.79 3.592 0l.094-.319a.873.873 0 0 1 1.255-.52l.292.16c1.64.893 3.434-.902 2.54-2.541l-.159-.292a.873.873 0 0 1 .52-1.255l.319-.094c1.79-.527 1.79-3.065 0-3.592l-.319-.094a.873.873 0 0 1-.52-1.255l.16-.292c.893-1.64-.902-3.433-2.541-2.54l-.292.159a.873.873 0 0 1-1.255-.52zm-2.633.283c.246-.835 1.428-.835 1.674 0l.094.319a1.873 1.873 0 0 0 2.693 1.115l.291-.16c.764-.415 1.6.42 1.184 1.185l-.159.292a1.873 1.873 0 0 0 1.116 2.692l.318.094c.835.246.835 1.428 0 1.674l-.319.094a1.873 1.873 0 0 0-1.115 2.693l.16.291c.415.764-.42 1.6-1.185 1.184l-.291-.159a1.873 1.873 0 0 0-2.693 1.116l-.094.318c-.246.835-1.428.835-1.674 0l-.094-.319a1.873 1.873 0 0 0-2.692-1.115l-.292.16c-.764.415-1.6-.42-1.184-1.185l.159-.291A1.873 1.873 0 0 0 1.945 8.93l-.319-.094c-.835-.246-.835-1.428 0-1.674l.319-.094A1.873 1.873 0 0 0 3.06 4.377l-.16-.292c-.415-.764.42-1.6 1.185-1.184l.292.159a1.873 1.873 0 0 0 2.692-1.115z"/>
        </svg>
      </div>
    </div>
    <p class="hint" id="hint-text" on:click={playBubbleEffect}>
      While you wait, you can tap this line once.
    </p>
  </section>
  <div id="bubble-layer" aria-hidden="true"></div>
</main>

<audio id="bubble-sound" preload="auto">
  <source src="/assets/bubble.mp3" type="audio/mpeg" />
</audio>

<style>
.page {
    min-height: 100vh;
    min-height: 100dvh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
}

.card {
    width: 100%;
    max-width: 920px;
    background: #ffffff;
    border-radius: 24px;
    border: 1px solid #e5e7eb;
    padding: 56px 48px 40px;
    text-align: center;
    position: relative;
    box-shadow: 0 30px 80px rgba(15, 23, 42, 0.15);
}

.title {
    font-size: 2.6rem;
    font-weight: 600;
    letter-spacing: 0.01em;
    margin-bottom: 20px;
}

.subtitle {
    font-size: 1.18rem;
    line-height: 1.85;
    color: #4b5563;
    max-width: 720px;
    margin: 0 auto 42px;
}

.gear-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 30px;
    color: #6b7280;
}

.gear-wrap {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 64px;
    height: 64px;
    transition: transform 0.2s ease;
}

.gear-wrap svg {
    width: 100%;
    height: 100%;
    animation: gear-rotate 3.5s linear infinite;
}

.gear-icon:hover .gear-wrap {
    transform: scale(1.5);
}

.hint {
    font-size: 0.96rem;
    color: #9ca3af;
    cursor: pointer;
    user-select: none;
    margin-top: 10px;
    transition: color 0.2s ease, transform 0.1s ease;
}

.hint:hover {
    color: #6b7280;
    transform: translateY(-1px);
}

@keyframes gear-rotate {
    to {
        transform: rotate(360deg);
    }
}

@media (max-width: 768px) {
    .card {
        padding: 42px 22px 30px;
        border-radius: 20px;
    }

    .title {
        font-size: 2.1rem;
    }

    .subtitle {
        font-size: 1.02rem;
        margin-bottom: 34px;
        max-width: none;
    }

    .gear-icon svg {
        width: 52px;
        height: 52px;
    }
}

@media (max-width: 480px) {
    .card {
        padding: 34px 16px 26px;
        border-radius: 18px;
    }

    .title {
        font-size: 1.8rem;
    }

    .subtitle {
        font-size: 0.98rem;
    }
}
</style>
