<script>
  import Switch from './Switch.svelte';

  let password = "";
  let pwdLen = 12;

  let up = true;
  let low = true;
  let num = true;
  let sym = true;

  function generatePass(){

    var uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    var lowercase = "abcdefghijklmnopqrstuvwxyz"
    var numbers = "0123456789"
    var symbols = "(!#$%&')*+,-."
    var chrs = uppercase+lowercase+numbers+symbols;
    password = '';

    for (let i = 0; i < pwdLen; i++ ) {
      password += chrs.charAt(Math.floor(Math.random() * 
      chrs.length));
   }

   if(!up){
     password = password.toLowerCase();
   } 
   if(!low){
     for(let i = 0; i < pwdLen; i++){
       if(lowercase.includes(password[i])){
         if(up){
           password = password.toUpperCase();
         }
         else if(num){
           password = password.replace(password[i], numbers.charAt(Math.floor(Math.random()*10)));
         }
         else if(sym){
          password = password.replace(password[i], symbols.charAt(Math.floor(Math.random()*symbols.length)));
         }
       }
     }
    }
     if(!num){
      for(let i = 0; i < pwdLen; i++){
       if(numbers.includes(password[i])){
        if(low){
          password = password.replace(password[i], lowercase.charAt(Math.floor(Math.random()*lowercase.length)));
         }
         else if(up){
           password = password.replace(password[i], uppercase.charAt(Math.floor(Math.random()*uppercase.length)));
         }
         else if(sym){
          password = password.replace(password[i], symbols.charAt(Math.floor(Math.random()*symbols.length)));
         }
      } 
     }
    }
     if(!sym){
      for(let i = 0; i < pwdLen; i++){
       if(symbols.includes(password[i])){
         console.log(password[i]);
        if(low){
          password = password.replace(password[i], lowercase.charAt(Math.floor(Math.random()*lowercase.length)));
         }
         else if(up){
           password = password.replace(password[i], uppercase.charAt(Math.floor(Math.random()*uppercase.length)));
         }
         else if(num){
          password = password.replace(password[i], numbers.charAt(Math.floor(Math.random()*10)));
         }
       }
      }
    }
    if(!up && !low && !num && !sym){
      password = "";
    }
  }

</script>

<body class="body-bg min-h-screen pt-12 md:pt-20 pb-6 px-2 md:px-0" style="font-family:'Lato',sans-serif;">

  <main class="bg-white max-w-md mx-auto p-8 md:p-12 my-10 rounded-lg shadow-2xl">
      <section>
          <h3 class="font-bold text-2xl">Super strong passwords with zero effort</h3>
          <p class="text-gray-600 pt-2">We can further improve your digital security with our free random password
            generator.</p>
      </section>

      <section class="mt-10">
          <div class="flex flex-col">
              <div class="mb-6 pt-3 rounded bg-purple-800 h-12">
                  <div class="block text-white font-bold text-center mb-3">{password}</div>
              </div>
              <div class="mb-6 pt-3 rounded bg-gray-200">
                <div class="block text-gray-700 text-sm font-bold mb-2 ml-3">
                  <p>Password length: {pwdLen}</p>
                  <input type="range" bind:value={pwdLen} min="4" max="64" class="range range-primary" style="width: 96%;"/>
                </div>
              </div>

              <div class="mb-6 h-10 flex rounded items-center justify-center bg-gray-200">
                <h3 class="pr-5">Uppercase</h3>
                <Switch bind:checked="{up}" id="up"/>
              </div>
              <div class="mb-6 h-10 flex rounded items-center justify-center bg-gray-200">
                <h3 class="pr-5">Lowercase</h3>
                <Switch bind:checked="{low}" id="low"/>
              </div>
              <div class="mb-6 h-10 flex rounded items-center justify-center bg-gray-200">
                <h3 class="pr-5">Numbers</h3>
                <Switch bind:checked="{num}" id="num"/>
              </div>
              <div class="mb-6 h-10 flex rounded items-center justify-center bg-gray-200">
                <h3 class="pr-5">Special characters</h3>
                <Switch bind:checked="{sym}" id="sym"/>
              </div>
              <button on:click={() => generatePass()} class="bg-purple-600 hover:bg-purple-700 text-white font-bold py-2 rounded shadow-lg hover:shadow-xl transition duration-200" type="submit">GENERATE PASSWORD</button>
          </div>
      </section>
  </main>
</body>

<style lang="postcss" global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>