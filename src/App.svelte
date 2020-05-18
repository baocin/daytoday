<script>
	import Gun from 'gun/gun'
	import SEA from 'gun/sea'

	// 'http://localhost:8765/gun', 'https://gunjs.herokuapp.com/gun', 
	// var gun = Gun(['https://gundb.home.steele.red/gun']); // Looks like it works... 
	var gun = Gun(['http://localhost:8765/gun']);
	console.log(gun)
	console.log(SEA)
	var user = gun.user();

	let say = "";
	let alias = "";
	let pass = "";

	let loggedInMessage = "";


    function createUser(e){
		console.log(alias, pass);
      	user.create(alias, pass);
    }

    function signIn(e){
	  console.log(alias, pass);
	  user.auth(alias, pass);
	  loggedInMessage = user.is ? "User is logged in" : "User is logged out"
	}

	function handleSpeak(e){
	  if(!user.is){ return }
	  console.log(say)
	//   const say = document.getElementById('say').value;
	  user.get('said').set(say);
	  say = "";
	  console.log(user.get('said'))
	}

	function UI(say, id){
    //   var li = $('#' + id).get(0) || $('<li>').attr('id', id).appendTo('ul');
	//   $(li).text(say);
		console.log(say)
	};
	
	gun.on('auth', function(){
	//   $('#sign').hide();
		console.log("AUTH triggered")
      user.get('said').map().once(UI);
    });
	
// 	var SEA = Gun.SEA;
// 	;(async () => {
// var pair = await SEA.pair();
// var enc = await SEA.encrypt('hello self', pair);
// var data = await SEA.sign(enc, pair);
// console.log(data);
// var msg = await SEA.verify(data, pair.pub);
// var dec = await SEA.decrypt(msg, pair);
// var proof = await SEA.work(dec, pair);
// var check = await SEA.work('hello self', pair);
// console.log(dec);
// console.log(proof === check);
// // now let's share private data with someone:
// var alice = await SEA.pair();
// var bob = await SEA.pair();
// var enc = await SEA.encrypt('shared data', await SEA.secret(bob.epub, alice));
// await SEA.decrypt(enc, await SEA.secret(alice.epub, bob));
// // `.secret` is Elliptic-curve Diffie–Hellman
// })();
</script>

<div>

<h1>Todo</h1>
{loggedInMessage}

<form id="sign">
	<input id="alias" placeholder="username" bind:value={alias}>
	<input id="pass" type="password" placeholder="passphrase" bind:value={pass}>
	<input on:click|preventDefault={signIn} type="submit" value="sign in">
	<input on:click={createUser} type="button" value="sign up">
</form>

<ul></ul>

<form id="said" on:submit|preventDefault={handleSpeak}>
	<input id="say" bind:value={say}>
	<input id="speak" type="submit" value="speak">
</form>

</div>
