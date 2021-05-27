<!DOCTYPE html>
<html>
        <meta charset="utf-8">
        <title>my webpage</title>
        <style>
        body {
            font-family: serif;/*Font*/
            background: white;/*Background color*/
            color: black;/*Text color*/
        }
        .header {
        background-color: rgb(179, 179, 179);/*Header background color*/
        text-align: center;
        padding: 1px;
        }
        ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
        }
        li {
        display: inline;
        }
        ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
        overflow: hidden;
        background-color: rgb(0, 0, 255);/*Navigation-bar background color*/
        }

        li {
            float: left;
        }
    
        li a {
          display: block;
          color: white;
          text-align: center;
          padding: 10px 10px;
          text-decoration: none;
        }
    
        li a:hover {
        background-color: #111;
        }
        
        h1 {
            font-size: 50px;
        }
        
        h2 {
            
        }
        
        h4 {
            font-size: 13px;
        }
        
        p {
            
        }
        
        .edge {
            margin: 25px;
        }
        
        button {
            position: fixed;
            bottom: 10px;
            right: 10px;
            z-index: 99;
            border: none;
            outline: none;
            background-color: rgb(153, 153, 153);/*Changes button's backgrou color*/
            cursor: pointer;
            padding: 7px;
            border-radius: 10px;
        }
        
        pj {
            float: left;
        }
        
        img {
            height: 40px;
            width: 40px;
        }
        
    </style>
    </head>
    <body>
    <script>
        var page = "platformer";
var blocks = [];
var portal = [];
var spikes = [];
var messages = ["Your Imagination", "Is the worlds creativity", "Your Imagination", "Is Endless", "And your Imagination", "Is the worlds biggest oppurtunity", "How did we start to code?", "Sal's Imagination", "How did I make this?", "My imagination", "What you do", "Changes the world", "And that is the reason of Imagination", "And that is why you don't Give Up!", "Your Ideas are always the best", "No one can stop you!", "Not your friends", "Not your cousins", "Not even your Parents!", "And that is the reason of Imagination", "To do what you want to do", "To change the world!", "Your Imagination"];
var levelNum = 0;
var deaths = 0;

//This key stuff is so that you can press multiple keys at once
var keys = [];
keyPressed = function() {
    keys[keyCode] = true;
};
keyReleased = function() {
    keys[keyCode] = false;
};

var rectCollide = function(rect1, rect2) {
    return rect1.x + rect1.w > rect2.x && rect1.x - rect2.w < rect2.x && rect1.y + rect1.h > rect2.y && rect1.y - rect2.h < rect2.y;
};//rect collide for blocks and portal
var collide = function(rect1, rect2, xs, ys) {
    if(rectCollide(rect1, rect2)) {//if the player is touching the block
        if(ys > 0) {//if it is falling
            rect1.y = rect2.y - rect1.h;//keeps it on top of the block
            rect1.ySpeed = 0;
            rect1.canJump = true;//makes sure it can jump
        }
        if(ys < 0) {//if it is rising
            rect1.y = rect2.y + rect2.h;//keeps it below the block
            rect1.ySpeed = 0;
        }
        if(xs > 0) {//if it is going right
            rect1.x = rect2.x - rect1.w;//keeps it on the right side of the block
            rect1.xSpeed = 0;
        }
        if(xs < 0) {//if it is going left
            rect1.x = rect2.x + rect2.w;//keeps it on the left side of the block
            rect1.xSpeed = 0;
        }
    }
};//rect Collide
var gravity = 0.3;//The amount of gravity
var jumpHeight = 7;//How high the player jumps
var char = {
    // values to control the character
    startX: 0,//return to x position
    startY: 0,//return to Y position
    x: 0,
    y: 0,
    w: 20,
    h: 20,
    canJump: false,
    xSpeed : 0,
    ySpeed : 0,
    moveR : 0,
    // a function to display the character
    display : function() {
	fill(0, 0, 0);
    noStroke();
    rect(this.x, this.y, this.w, this.h,this.r);
    
        
    },
    
    // function for player movement
    move : function() {
    this.x = constrain(this.x,5,376);
    if(this.ySpeed < 12) {//keeps the player from falling too fast
        this.ySpeed += gravity;//This is what makes the player fall
    }
    
    if(keys[LEFT]) {
        this.xSpeed = -3;//sets the x-speed
        this.moveR = -3;
    }else if(keys[RIGHT]) {
        this.xSpeed = 3;
        this.moveR = 3;
    }else {
        this.xSpeed = 0;
    }
    
    if(keys[UP]&&this.canJump) {
        this.ySpeed = -jumpHeight;//jumps
    }
    this.canJump = false;//Makes sure you can't float away into the sky
        // x collision
	this.x += this.xSpeed;//This is when the player moves 
    for(var i = 0; i<blocks.length; i++) {
        collide(this, blocks[i], this.xSpeed, 0);
    }
        
    // y-direction movement
    this.y += this.ySpeed;
        
        // y collision
    for(var i = 0; i<blocks.length; i++) {
        collide(this, blocks[i], 0, this.ySpeed);
    }
    

    }
};//character object

var Block = function(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    
};
Block.prototype.display = function() {

    noStroke();
    fill(255, 255, 255);
    rect(this.x, this.y, this.w, this.h);
};//display

var Spike = function (x,y,w,h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    
};
Spike.prototype.display = function () {
    fill(255, 255, 255);
    noStroke();
    noFill();
    rect(this.x,this.y,this.w,this.h,0);
    fill(255, 255, 255);
    noStroke();
    triangle(this.x +0, this.y+20, this.x+20, this.y + 20, this.x+10, this.y);
    
};//display

Spike.prototype.collide = function() {
    if(rectCollide(this, char)) {
        char.x = char.startX;
        char.y = char.startY;
        deaths++;
    }
};
    
var Portal = function(x,y,w,h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    
};
var addBlocks;

Portal.prototype.display = function () {
    noFill();
    rect(this.x,this.y,this.w,this.h);
    for(var i = 0; i < 20; i+= 2) {
    fill(0, 225, 255, 9);
    ellipse(this.x+10, this.y+10, i+this.w, i+this.h);
    }

};//display
Portal.prototype.collide = function() {
    if(rectCollide(this, char)) {
        levelNum++;
        addBlocks();
        
    }
};

var levels = [
    //lvl 1
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "c------------------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 2
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "-------------------p",
    "----bbbbbbbbbbbbbbbb",
    "c-------------------",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 3
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "---------bbbbbbbbb--",
    "--------b-----------",
    "----bbbbb-bbbbbbbbbb",
    "c--b---------------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 4
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "c---b---ss---------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 5
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "c--s--s--s--s--s-p",
    "bbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 6
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "-----------bbb--bbbb",
    "-----------brr--b---",
    "---------bbb----b---",
    "------ss--bb--bbb---",
    "----bbbbssbb--b-----",
    "c-bbbbbbbbbb-------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 7
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "----b---------------",
    "----b----ss--------c",
    "----b--bbbbbbbbbbbbb",
    "----b--b------rr----",
    "----b--b-----------p",
    "----b--bb----------b",
    "----b------ss-----sb",
    "----bbbbbbbbbbbbbabb",
    ],
    
    //lvl 8
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "bbbbbbbbbbbbbbbbbbbb",
    "----b---------------",
    "----b--------------p",
    "-------bbbbbbbbbbbbb",
    "--bbbbbb------------",
    "--------------------",
    "bbbbbbbbbbbbbbbbbb--",
    "c-------------------",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 9
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "------------ss-----p",
    "----------bbbbbbbbbb",
    "--------b-rrrrrrrrrr",
    "------b-------------",
    "----b---------------",
    "c-b--sssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 13
    [
    "b------------------b",
    "b------------------b",
    "b-------------bbbbbb",
    "b------------------b",
    "b------------------b",
    "b------------------b",
    "b-----b------------b",
    "b------------------b",
    "b--bbbbb------bbbb-b",
    "b------------------b",
    "b-bbbbbb------bb--pb",
    "b-----------------bb",
    "bb---------------b-b",
    "b-b-------------b--b",
    "b--------------b---b",
    "b-------------b----b",
    "b-----b------------b",
    "b---b--------------b",
    "c-b--ssssssssssssssb",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 14
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "----------bbbbbbbbbb",
    "----------rrrrrrrrrr",
    "--------b-----------",
    "------b-----bbbbbb--",
    "----b------b--------",
    "c---------b--------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 15
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "c------------------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 16
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "------------ss-----p",
    "----------bbbbbbbbbb",
    "--------brrrrrrrrrrr",
    "------b-------------",
    "----b---------------",
    "c-b--sssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 17
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------b-------p---",
    "------b-------------",
    "----b---------------",
    "c-b-----------------",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 18
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "p-------------------",
    "bbbbbbbbbbbbbbbbb---",
    "rrrrrrrrrrrrrrrr----",
    "-------------------b",
    "--------------------",
    "----------------b---",
    "--------------------",
    "-------------------b",
    "--------------------",
    "----------------b---",
    "c------------------b",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 19
    [
    "--------------------",
    "--------------------",
    "c-------------------",
    "b-------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "----------p---------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "ssssssssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 20
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "------------ss-----p",
    "----------bbbbbbbbbb",
    "--------brrrrrrrrrrr",
    "------b-------------",
    "----b---------------",
    "c-b--sssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 21
    [
    "--------------------",
    "-------------------p",
    "------------bbbbbbbb",
    "--------------------",
    "bbbbbbbbb-----------",
    "--------------------",
    "------------bbbbbbbb",
    "--------------------",
    "bbbbbbbbb-----------",
    "------------bbbbbbbb",
    "--------------------",
    "---------c----------",
    "---------bb---------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "ssssssssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 22
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "----------b--------p",
    "----------bbbbbbbbbb",
    "--------------------",
    "------b-------bbbbb-",
    "-------------bb----b",
    "c--------b----b----p",
    "bbbb----------bbbbbb",
    ],
    
    //lvl 23
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "---------c----------",
    "--------bbb---------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------p-----------",
    "--------------------",
    "--------------------",
    "ssssssssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 24
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "-----------b--------",
    "-----------b--------",
    "-----------bss-----p",
    "----------bbbbbbbbbb",
    "--------brrrrrrrrrrr",
    "------b-------------",
    "----b---------------",
    "c-b---ssssssssssssss",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 25
    [
    "--------------------",
    "--------------------",
    "-------------------p",
    "--------------b---bb",
    "--------------------",
    "-----------------b--",
    "--------------------",
    "----------b--b------",
    "--------b-----------",
    "------b-------------",
    "--------------------",
    "bb------------------",
    "---b----------------",
    "-----b--------------",
    "-------b------------",
    "---------b----------",
    "-----------b--------",
    "-------------b------",
    "c-------------------",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
    //lvl 26
    [
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "--------------------",
    "c------------------p",
    "bbbbbbbbbbbbbbbbbbbb",
    ],
    
//going to do 26 lvls!    
];//levels
    
    var addBlocks = function() {
    blocks = [];
    portal = [];
    spikes = [];

for(var j = 0; j < levels[levelNum].length; j++) {
        for(var i = 0; i < levels[levelNum][j].length; i++) {
            switch(levels[levelNum][j][i]) {
                case "b":
                    /** adds blocks to the block array*/
                    blocks.push(new Block(i * 20, j * 20,20,20));
                break;
                 
                case "s":
                    spikes.push(new Spike(i*20,j*20,20,20));
                break;
                
                case "p":
                    portal.push(new Portal(i*20,j*20,20,20));
                break;
        
                /** this is the position of the player */
                
                case "c":
                    char.x= i * 20;
                    char.y = j * 20;
                    
                    char.startX = i*20;
                    char.startY = j*20;
                break;


            }
        }
    }
};

addBlocks();

var platformer = function () {
    background(150, 0, 150);
    char.display();
    char.move();
    if(char.y>400) {
        char.x = char.startX;
        char.y = char.startY;
        
    }
    for(var i = blocks.length - 1; i >= 0; i--) {
        blocks[i].display();
    }
        for(var i = spikes.length - 1; i >= 0; i--) {
        spikes[i].display();
        spikes[i].collide();
    }
        for(var i = portal.length - 1; i >= 0; i--) {
        portal[i].display();
        portal[i].collide();
    }
    fill(255, 255, 255);
    textSize(20);
    text("Deaths - " + deaths,60,10,300,300);
    textAlign(CENTER,TOP);
    text(messages[levelNum],9,106,400,400);

};

var applyGame = function () {
    switch(page) {
        
        case "platformer":
            cursor("default");
            platformer();
        break;
        
    }
    
};

draw = function() {
    applyGame();
};
    
</script>
        <iframe src="https://docs.google.com/forms/d/e/1FAIpQLSfdTilBLm29F03AtkqVQXR7uoWCQ143qOczcWu0Udqyztm84g/viewform?embedded=true" width="640" height="1335" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe><head>
    <div class="header">
  <h1>MAIN TITLE</h1>
</div>
    <ul>
        <li><a href="link"><img src = "https://www.kasandbox.org/programming-images/avatars/aqualine-ultimate.png"></a></li>
        <li><a href="#link1">Link 1</a></li>
        <li><a href="#link2">Link 2</a></li>
        <li><a href="#link3">Link 3</a></li>
        <li><a href="#link4">Link 4</a></li>
    </ul>
    <div class="edge">
    
    <h2 id = "link1">Title 1</h2>
    <div>
    <p>Text.</p>
    </div>
    
    <h2 id = "link2">Title 2</h2>
    <div>
    <p>More text</p>
    </div>
    
    <h2 id = "link3">Title 3</h2>
    <div>
    <p>TEXT <a href = "link">link.</a> More Text
    </p>
    </div>
    
    <h2 id = "link4">Title 4</h2>
    <p>Text.</p>
    
    <button><a href = "#top"><img src = "https://upload.wikimedia.org/wikipedia/commons/9/9e/U%2B2191.svg"></a></button>
    </div>
    </body>
</html>
