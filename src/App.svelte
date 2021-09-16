<script>
  import Switch from "./Switch.svelte";
  import Clipboard from "svelte-clipboard";
  import zxcvbn from "zxcvbn";
 
  let pwd = "",
		  msg = ["Too Weak 🤒", "Weak ☠️", "Good 👍", "Strong 💪", "Super Strong 🤖"],
      clr = ["progress-error","progress-error","progress-warning","progress-secondary","progress-success"];

  var isStrong = "";

  var pwdLen = 9;
  var pwdStr = 0;
  var pwdStrText = "";
  var pwdStrColor = "progress-error";

  let tooltipText = "Click to copy!"

  let up = true;
  let low = true;
  let num = true;
  let sym = false;

  $:generatePass(pwdLen);

  function pwdStrHandler(){
    var score = zxcvbn(pwd).score;
    pwdStr = score;
    pwdStrText = msg[score];
    pwdStrColor = clr[score];
  }

  function tooltipTextHandler(){
    if(tooltipText = "Click to copy!"){
      tooltipText = "Copied!";
    }
    else{
      tooltipText = "Click to copy!";
    }
  }

  function generatePass(pwdLen) {
    pwd = "";
    const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const lowercase = "abcdefghijklmnopqrstuvwxyz";
    const numbers = "0123456789";
    const symbols = "._-!?/(),";

    let active = [];
    let ac = 0;

    if (up) {
      active.push(uppercase);
      ac++;
    }

    if (low) {
      active.push(lowercase);
      ac++;
    }

    if (num) {
      active.push(numbers);
      ac++;
    }
    if (sym) {
      active.push(symbols);
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

    if (pwd.length < pwdLen && pwd.length != 0) {
      const randomElement = active[Math.floor(Math.random() * active.length)];
      pwd += randomElement.charAt(Math.random() * randomElement.length);
    }
    pwd = pwd.split("");
    for (let i = 0; i < pwd.length; i++) {
      var j = Math.floor(Math.random() * pwd.length);
      var tmp = pwd[i];
      pwd[i] = pwd[j];
      pwd[j] = tmp;
    }
    pwd = pwd.join("");
    pwdStrHandler();
  }
</script>

<link rel="stylesheet" href="https://pro.fontawesome.com/releases/v5.10.0/css/all.css" integrity="sha384-AYmEC3Yw5cVb3ZcuHtOA93w35dYTsvhLPVnYs9eStHfGJvOvKxVfELGroGkvsg+p" crossorigin="anonymous"/>

<svelte:head>
  <link rel="icon" type="image/png" href="icons/favicon.png" />
</svelte:head>

<body
  class="bg-gradient-to-br from-bright-turquoise-300 to bg-cornflower-blue-500 bg-size-200 bg-pos-0 hover:bg-pos-100 min-h-screen pt-12 md:pt-20 pb-6 px-2 md:px-0"
  style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif">
  <main
    class="bg-white max-w-md mx-auto p-8 md:p-12 my-10 rounded-lg shadow-2xl" style="backdrop-filter: blur(20px)">
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
      <div class="block text-gray-400 text-xs font-semibold mb-5">
        <p class="pb-2">{pwdStrText}</p>
        <progress value="{pwdStr}" min="0" max="4" class="progress {pwdStrColor}" style=";"></progress>
      </div>
      <div class="flex flex-col">
        <div data-tip="{tooltipText}" class="tooltip tooltip-primary">
          <Clipboard text="{pwd}" let:copy on:copy={() => {
          }}>
        <button on:click={copy} on:click={() => tooltipTextHandler()} class="w-full mb-6 pt-3 bg-gradient-to-br from-bright-turquoise-500 to bg-cornflower-blue-500 hover:shadow-xl transition-all duration-500 hover:bg-gradient-to-br hover:from-bright-turquoise-500 to hover:bg-cornflower-blue-600 bg-size-200 bg-pos-0 hover:bg-pos-100 text-white font-bold py-2 rounded shadow-lg h-12">
          <div class="block text-white font-semibold items-center text-center mb-3">
            {pwd}
          </div>
        </button>
      </Clipboard>
        </div>
        <div class="mb-6 pt-3 rounded-lg bg-gray-50 shadow-inner">
          <div class="block text-gray-400 text-xs font-semibold mb-2 ml-3">
            <p class="pb-2">Password length: {pwdLen}</p>
            <input type="range" bind:value={pwdLen} on:hashchange={() => generatePass(pwdLen)} min="4" max="32" class="range range-primary" style="width: 96%;"/>
          </div>
        </div>

        <div class="mb-2 h-10 flex rounded-lg items-center justify-between bg-gray-50 shadow-inner">
          <h3 class="pr-5 pl-5">Uppercase</h3>
          <Switch bind:checked={up} id="up" />
        </div>
        <div class="mb-2 h-10 flex rounded-lg items-center justify-between bg-gray-50 shadow-inner">
          <h3 class="pr-5 pl-5">Lowercase</h3>
          <Switch bind:checked={low} id="low" />
        </div>
        <div class="mb-2 h-10 flex rounded-lg items-center justify-between bg-gray-50 shadow-inner">
          <h3 class="pr-5 pl-5">Numbers</h3>
          <Switch bind:checked={num} id="num" />
        </div>
        <div class="mb-6 h-10 flex rounded-lg items-center justify-between bg-gray-50 shadow-inner">
          <h3 class="pr-5 pl-5">Special characters</h3>
          <Switch bind:checked={sym} id="sym" />
        </div>
        <button on:click={() => generatePass(pwdLen)} on:click={() => { tooltipText = 'Copy to clipboard!' }}
          class="bg-gradient-to-br from-bright-turquoise-500 to bg-cornflower-blue-500 hover:shadow-xl transition-all duration-500 hover:bg-gradient-to-br hover:from-bright-turquoise-500 to hover:bg-cornflower-blue-600 bg-size-200 bg-pos-0 hover:bg-pos-100 text-white font-semibold py-2 rounded-lg shadow-lg" type="submit">GENERATE PASSWORD</button
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
