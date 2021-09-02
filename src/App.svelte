<script>
  import Switch from "./Switch.svelte";
  import Clipboard from "svelte-clipboard";
  

  let pwd = "";
  let pwdLen = 8;

  let up = true;
  let low = true;
  let num = true;
  let sym = false;

  function generatePass() {
    pwd = "";
    const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const lowercase = "abcdefghijklmnopqrstuvwxyz";
    const numbers = "0123456789";
    const symbols = "(!#$%&')*+,-.";

    let active = [];
    let ac = 0;

    if (up) {
      active.push("uppercase");
      ac++;
    }

    if (low) {
      active.push("lowercase");
      ac++;
    }

    if (num) {
      active.push("numbers");
      ac++;
    }
    if (sym) {
      active.push("symbols");
      ac++;
    }

    if (up) {
      for (let i = 0; i < Math.floor(pwdLen / ac); i++) {
        pwd += uppercase.charAt(
          Math.floor(Math.random() * uppercase.length)
        );
      }
    }

    if (low) {
      for (let i = 0; i < Math.floor(pwdLen / ac); i++) {
        pwd += lowercase.charAt(
          Math.floor(Math.random() * lowercase.length)
        );
      }
    }

    if (num) {
      for (let i = 0; i < Math.floor(pwdLen / ac); i++) {
        pwd += numbers.charAt(Math.floor(Math.random() * numbers.length));
      }
    }

    if (sym) {
      for (let i = 0; i < Math.floor(pwdLen / ac); i++) {
        pwd += symbols.charAt(Math.floor(Math.random() * symbols.length));
      }
    }

    if (pwd.length < pwdLen) {
      const randomElement = active[Math.floor(Math.random() * active.length)];
      pwd += randomElement.charAt(
        Math.floor(Math.random() * randomElement.length)
      );
    }
    pwd = pwd.split("");
    for (let i = 0; i < pwd.length; i++) {
      var j = Math.floor(Math.random() * pwd.length);
      var tmp = pwd[i];
      pwd[i] = pwd[j];
      pwd[j] = tmp;
    }
    pwd = pwd.join("");
  }
</script>

<link rel="stylesheet" href="https://pro.fontawesome.com/releases/v5.10.0/css/all.css" integrity="sha384-AYmEC3Yw5cVb3ZcuHtOA93w35dYTsvhLPVnYs9eStHfGJvOvKxVfELGroGkvsg+p" crossorigin="anonymous"/>

<body
  class="body-bg min-h-screen pt-12 md:pt-20 pb-6 px-2 md:px-0"
  style="font-family:'Lato',sans-serif;"
>
  <main
    class="bg-white max-w-md mx-auto p-8 md:p-12 my-10 rounded-lg shadow-2xl"
  >
    <section>
      
      <h3 class="font-bold text-2xl">
        Super strong passwords with zero effort
      </h3>
      <p class="text-gray-600 pt-2">
        We can further improve your digital security with our free random
        password generator.
      </p>
    </section>

    <section class="mt-10">
      <div class="flex flex-col">
        <Clipboard text="{pwd}" let:copy on:copy={() => {
          console.log('Has Copied');
        }}>
        <button on:click={copy} class="mb-6 pt-3 rounded bg-purple-600 hover:bg-purple-700 hover:shadow-xl transition duration-200 h-12">
          <div class="block text-white font-bold items-center text-center mb-3">
            {pwd}
          </div>
        </button>
      </Clipboard>
        <div class="mb-6 pt-3 rounded bg-gray-200">
          <div class="block text-gray-700 text-sm font-bold mb-2 ml-3">
            <p>Password length: {pwdLen}</p>
            <input
              type="range"
              bind:value={pwdLen}
              min="4"
              max="32"
              class="range range-primary"
              style="width: 96%;"
            />
          </div>
        </div>

        <div
          class="mb-2 h-10 flex rounded items-center justify-between bg-gray-200"
        >
          <h3 class="pr-5 pl-5">Uppercase</h3>
          <Switch bind:checked={up} id="up" />
        </div>
        <div
          class="mb-2 h-10 flex rounded items-center justify-between bg-gray-200"
        >
          <h3 class="pr-5 pl-5">Lowercase</h3>
          <Switch bind:checked={low} id="low" />
        </div>
        <div
          class="mb-2 h-10 flex rounded items-center justify-between bg-gray-200"
        >
          <h3 class="pr-5 pl-5">Numbers</h3>
          <Switch bind:checked={num} id="num" />
        </div>
        <div
          class="mb-6 h-10 flex rounded items-center justify-between bg-gray-200"
        >
          <h3 class="pr-5 pl-5">Special characters</h3>
          <Switch bind:checked={sym} id="sym" />
        </div>
        <button
          on:click={() => generatePass()}
          class="bg-purple-600 hover:bg-purple-700 text-white font-bold py-2 rounded shadow-lg hover:shadow-xl transition duration-200"
          type="submit">GENERATE PASSWORD</button
        >
      </div>
    </section>
  </main>
</body>

<style lang="postcss" global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
