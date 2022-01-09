<template>
  <div>
    <header></header>
    <main>
      <section>
        <svg
          width="22"
          height="29"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M5.5 2H10v7H5.487A3.5 3.5 0 0 1 5.5 2ZM12 0H5.5a5.5 5.5 0 0 0-3.163 10A5.493 5.493 0 0 0 0 14.5c0 1.86.923 3.505 2.337 4.5A5.5 5.5 0 0 0 5.5 29H6a6 6 0 0 0 6-6v-5.337A5.5 5.5 0 1 0 19.663 10 5.5 5.5 0 0 0 16.5 0H12Zm4.5 11h-.017a3.5 3.5 0 1 0 .03 0H16.5Zm0-2H12V2h4.5a3.5 3.5 0 0 1 .017 7H16.5Zm-11 2H10v7H5.487a3.5 3.5 0 0 1 0-7H5.5Zm-.013 9a3.5 3.5 0 0 0 .013 7H6a4 4 0 0 0 4-4v-3H5.487Z"
            fill="#323232"
          />
        </svg>
        <div class="loader">
          <span class="progress" />
        </div>
        <div class="buttons">
          <button class="button-secondary">Save Figjam</button>
          <button>Continue to Editor</button>
        </div>
        <footer>
          You can draw on the canvas to get started your file is loaded
        </footer>
      </section>
    </main>
  </div>
</template>

<style lang="scss" scoped>
  header {
    width: 100vw;
    height: 4rem;
    background: #2c2c2c;
  }

  main {
    width: 100vw;
    height: calc(100vh - 4rem);
    background: #e5e5e5;
  }

  section {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 2.4rem;
  }

  .loader {
    position: relative;
    margin-top: 1.2rem;
    margin: 1.2rem 0;
    border-radius: 10px;
    width: 18rem;
    height: 0.6rem;
    background: #fff;
    overflow: hidden;
  }

  .progress {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 6px;
    background: #333333;
    transform: scaleX(0);
    transform-origin: left;
  }

  .buttons {
    margin-top: 0.6rem;
  }

  button {
    margin-top: 1.2rem;
    border-radius: 6px;
    padding: 1.2rem 1.2rem;
    background: #18a0fb;
    color: #fff;
    font-size: 1.6rem;
  }

  .button-secondary {
    margin-right: 1.2rem;
    background: none;
    color: var(--gravity);
  }

  footer {
    position: absolute;
    bottom: 2rem;
    border-radius: 6px;
    padding: 0.8rem 1.2rem;
    background: var(--gravity);
    color: #fff;
    font-weight: 500;
    font-size: 1.4rem;
    text-align: center;
    box-shadow: 0 4px 22px rgba(0, 0, 0, 0.24);
  }
</style>

<script>
  export default {
    mounted() {
      gsap.to(".progress", {
        scaleX: 1,
        duration: 8,
        ease: Power4.easeIn,
      })

      gsap.from(".buttons", {
        opacity: 0,
        duration: 0.3,
        delay: 8,
        ease: Power4.easeIn,
      })

      const canvas = document.createElement("canvas")
      document.querySelector("main").appendChild(canvas)

      canvas.style.cssText = `
        position: fixed;
        z-index: 99999;
      `

      const ctx = canvas.getContext("2d")
      resize()

      const pos = { x: 0, y: 0 }

      window.addEventListener("resize", resize)
      document.addEventListener("mousemove", draw)
      document.addEventListener("mousedown", setPosition)
      document.addEventListener("mouseenter", setPosition)

      function setPosition(e) {
        pos.x = e.clientX
        pos.y = e.clientY - 180
      }

      function resize() {
        ctx.canvas.width = window.innerWidth
        ctx.canvas.height = window.innerHeight
      }

      function draw(e) {
        if (e.buttons !== 1) return

        ctx.beginPath() // begin

        ctx.lineWidth = 3
        ctx.lineCap = "round"
        ctx.strokeStyle = "#18A0FB"

        ctx.moveTo(pos.x, pos.y)
        setPosition(e)
        ctx.lineTo(pos.x, pos.y)

        ctx.stroke()
      }
    },
  }
</script>
