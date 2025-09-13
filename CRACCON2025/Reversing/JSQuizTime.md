When you open the challenge on browser, you would see that you're being asked a question.

If you open the page source, and take the first line of obfuscated JS code and paste it into a deobfuscator, (i.e. https://obf-io.deobfuscate.io/)
You can get the source code which looks like - 
```js
unction tungtungtungsahur() {
  if (document.getElementById("flag").value == atob("dGhlcmUgYXJlIG1hbnk=")) {
    document.getElementById('q2').removeAttribute("hidden");
    document.getElementById('q1').setAttribute("hidden", "hidden");
    document.getElementById("overlay").style.display = "block";
    document.getElementById("overlay").innerHTML = "<div class='correct'><h2>Right Answer!</h2></div>";
    document.getElementById("question").innerText = atob(atob("TWk0Z1NHOTNJRzFoYm5rZ1puVnVZM1JwYjI1eklHVjRhWE4wSUdsdUlIUm9hWE1nY1hWbGMzUnBiMjV1WVdseVpUOD0="));
  } else {
    document.getElementById("overlay").style.display = "block";
    document.getElementById("overlay").innerHTML = "<div class='incorrect'><h2>Wrong Answer!</h2></div>";
  }
}
function closeOverlay() {
  document.getElementById("overlay").style.display = "none";
}
function aatob(_0xed91c) {
  return atob(atob(_0xed91c));
}
function BrrBrrPatapim() {
  if (document.getElementById("flag").value == "there are too many, I was lazy to count") {
    document.getElementById('q3').removeAttribute("hidden");
    document.getElementById('q2').setAttribute("hidden", "hidden");
    document.getElementById("overlay").style.display = "block";
    document.getElementById("overlay").innerHTML = "<div class='correct'><h2>Right Answer!</h2></div>";
    document.getElementById("question").innerText = atob("My4gV2hhdCBpcyB0aGUgZmxhZyBmb3IgdGhpcyBjaGFsbGVuZ2U/");
  } else {
    document.getElementById("overlay").style.display = "block";
    document.getElementById("overlay").innerHTML = "<div class='incorrect'><h2>Wrong Answer!</h2></div>";
  }
}
```

From here, you can get the answers to the first 2 questions, by performing the atob() function, used to replace the innerText of question. 
Then the 3rd question asks about what is the flag.
We still haven't seen the 2nd line of JS code that is obfuscated. Hence we deobfuscate it using a different tool (https://deobfuscate.io/)

This time there are more than 100 lines of code, hence we go step wise.

From the body of the webpage, we can see that the durability2() function is being called. This looks like - 
```js
const durability2 = () => {
  const _0xc4d181 = _0x49b3;
  const _0x45b12e = _0x9e8a02;
  if (bombardinocrocodilo(String(tralalelotralala(skjdhfk1(document[_0x45b12e(6)](_0xc4d181(27))[_0x45b12e(3)])))) == skibiditoilet) alert(_0x45b12e(4)); else alert(_0xc4d181(18));
};
```
if we try directly using the functions, we see - 
1.  `skjdhfk1()` is used to shuffle the characters within the text we submit.
2.  `tralalelotralala()` is used to convert text into a hex string.
3.  `bombardinocrocodilo()` is used to switch characters of hex string with other hexadecimal characters.

And finally a comparison is done with `skibiditoilet`.

We can assume that `skibiditoilet` is a string representing the flag. We can again make an assumption that this must be a long hex string based on how it was obfuscated.

If we go through the unobfuscated source, we see

```js
const _0x305f37 = ["360EatjDv", "2480226zXNgjG", "84029KdXlWL", "1119880iZYBXr", "toString", "5YTzOwD", "flag is correct!", "7gXRFQT", "length", "299c2a9926565c51932223992e935f292c599d5929fdfd57292d5590", "shift", "1131mQXOAT", "wrong flag, try again", "81sYbLRj", "2043895qRMLmN", "529590FzeOBV", "536260WmOkTr", "791872QCJxMk", "16BMawoF", "496010GEoQzF", "39KVHhtx", "flag", "16ZntvkA", "charCodeAt", "push", "8255172klMIbb", "1261107yjjydM", "14004990jEuWPF", "value", "70fFFOUR", "Invalid hex character: ", "join", "734rgrdvZ"];
```

In this, 1 string stands out well, which is - `"299c2a9926565c51932223992e935f292c599d5929fdfd57292d5590"`
This matches the description we had for `skibiditoilet`

now, to reverse this, we need to start with reversing `bombardinocrocodilo()`
This can be done using 
```js
function hexSubstituteReverse(hexString) {
  let result = "";
  const inverseMultiplier = 11;
  const mod = 16;

  for (let i = 0; i < hexString.length; i++) {
    const c = hexString[i];
    const digit = parseInt(c, 16);

    if (isNaN(digit)) {
      throw new Error(`Invalid hex character: ${c}`);
    }

    const originalDigit = (digit * inverseMultiplier) % mod;
    result += originalDigit.toString(16);
  }

  return result;
}
```

Then to reverse `tralalelotralala()` we just need to convert the resultant string from hex to ascii.
This can also be done using the below js function -

```js
function fromHex(h) {
    var s = ''
    for (var i = 0; i < h.length; i+=2) {
        s += String.fromCharCode(parseInt(h.substr(i, 2), 16))
    }
    return decodeURIComponent(escape(s))
}
```
Finally to reverse `skjdhfk1()`, we need to unshuffle it based on how it is shuffled. 
If the deobfuscation is done perfectly, we can see that the function performs operation as - 
```js
for (let i = 0; i < n; i++) {
  const newIndex = (i * k) % n;
  result[newIndex] = input[i];
}
```
Which resembles hashing however, we know that there must be no collisions since 1 index can only contain 1 character.
`k=5` and `n=28` from knowing this, we perform inverse modulus to get back the original arrangement.
This can be done using the function - 

```js
function unjumble28(jumbled) {
  const result = new Array(28);
  const kInverse = 17;
  const n = 28;
  
  for (let i = 0; i < n; i++) {
    const originalIndex = (i * kInverse) % n;
    result[originalIndex] = jumbled[i];
  }
  
  return result.join('');
}
```

Hence we get the flag as - 

`craccon{js_0bfusc4t3d_w311?}`

which can be given as input to question 3 and get it verfied that it is correct.
