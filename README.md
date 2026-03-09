<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Anonymous Chat Prototype</title>

<style>
body{
    margin:0;
    font-family: monospace;
    background:black;
    color:#00ff88;
    overflow:hidden;
}

/* Hacker background effect */
body::before{
    content:"";
    position:fixed;
    inset:0;
    background:radial-gradient(circle at center,#002200 0%,#000000 70%);
    opacity:0.8;
}

/* Chat container */
.chat-container{
    position:absolute;
    bottom:0;
    width:100%;
    background:#000;
    border-top:1px solid #00ff88;
    padding:10px;
    box-sizing:border-box;
}

/* Message display */
.messages{
    height:40vh;
    overflow-y:auto;
    padding:10px;
}

.msg{
    margin:5px 0;
}

/* Input area */
.input-area{
    display:flex;
    gap:10px;
}

input{
    flex:1;
    background:black;
    border:1px solid #00ff88;
    color:#00ff88;
    padding:8px;
    font-family:monospace;
}

button{
    background:black;
    border:1px solid #00ff88;
    color:#00ff88;
    padding:8px 15px;
    cursor:pointer;
}

button:hover{
    background:#003300;
}

/* Popup style */
.popup{
    position:fixed;
    background:black;
    border:1px solid #00ff88;
    color:#00ff88;
    padding:10px;
    box-shadow:0 0 10px #00ff88;
    font-size:18px;
    animation:glow 1s infinite alternate;
}

@keyframes glow{
    from{ box-shadow:0 0 5px #00ff88; }
    to{ box-shadow:0 0 20px #00ff88; }
}
</style>
</head>

<body>

<div class="messages" id="messages"></div>

<div class="chat-container">
<div class="input-area">
<input id="msgInput" placeholder="type message...">
<button onclick="sendMessage()">send</button>
</div>
</div>

<script>

const messages = document.getElementById("messages");
const input = document.getElementById("msgInput");

let popupMode = false;

/* Send message */
function sendMessage(){

    const text = input.value.trim();
    if(!text) return;

    const div = document.createElement("div");
    div.className = "msg";
    div.textContent = "> " + text;
    messages.appendChild(div);

    messages.scrollTop = messages.scrollHeight;

    if(/^(hi|hello)$/i.test(text)){
        startInfinitePopups();
    }

    input.value="";
}

/* Create popup */
function createPopup(){

    const popup = document.createElement("div");
    popup.className="popup";
    popup.textContent="i love you";

    popup.style.left=Math.random()* (window.innerWidth-150) + "px";
    popup.style.top=Math.random()* (window.innerHeight-100) + "px";

    document.body.appendChild(popup);

    /* Remove older ones to avoid memory crash */
    if(document.body.children.length > 200){
        document.body.removeChild(document.body.children[1]);
    }
}

/* Infinite popup generator (non-blocking) */
function startInfinitePopups(){

    if(popupMode) return;
    popupMode = true;

    function loop(){
        for(let i=0;i<8;i++){
            createPopup();
        }
        requestAnimationFrame(loop);
    }

    requestAnimationFrame(loop);
}

/* Enter key support */
input.addEventListener("keydown",e=>{
    if(e.key==="Enter"){
        sendMessage();
    }
});

</script>

</body>
</html>
