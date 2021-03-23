<!DOCTYPE html>
<html>
    <head>
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
