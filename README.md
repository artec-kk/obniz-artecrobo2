# Artec.js

Usage

Include obniz.js and artec.js

```
<script src="https://unpkg.com/obniz@2.2.0/obniz.js"></script>
<script src="https://cdn.jsdelivr.net/gh/artec-kk/obniz-artecrobo2@master/artec.js"></script>
```

```html
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <script src="https://obniz.io/js/jquery-3.2.1.min.js"></script>
  <script src="https://unpkg.com/obniz@2.2.0/obniz.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/artec-kk/obniz-artecrobo2@master/artec.js"></script>
</head>

<body>

  <div id="obniz-debug"></div>
  <input type="text" id="message" value="Helloこんにちは안녕하세요你好" /><button id="send">send</button>

  <script>

    var message = "Helloこんにちは안녕하세요你好";

    var stubit = new Artec.StuduinoBit('8269-6747');
    stubit.onconnect = async function () {
      stubit.display.on();

      const ctx = stubit.obniz.util.createCanvasContext(5, 5);

      let x = 5;
      stubit.obniz.repeat(async function () {

        ctx.fillStyle = "black";
        ctx.fillRect(0, 0, 5, 5);
        ctx.fillStyle = "gray";
        ctx.font = "9px sans-serif";
        ctx.fillText(message, x, 5);
        x--;

        draw(ctx);
      }, 500)

      function draw(ctx) {

        let width = 5;
        let height = 5;
        //1pxずつデータ通信をしないために一度offにする
        stubit.display.off();

        const imageData = ctx.getImageData(0, 0, width, height);
        const data = imageData.data;

        for (let i = 0; i < data.length; i += 4) {
          let index = parseInt(i / 4);
          let line = parseInt(index / width);
          let col = parseInt(index - line * width);
          stubit.display.setPixcel(col, line, [data[i], data[i + 1], data[i + 2]]);
        }
        stubit.display.on();
      }

      $("#send").click(async () => {
        message = $("#message").val();
        x = 5;
      })
    }


    async function ledBlink() {
      while (1) {
        stubit.led.on();
        await stubit.wait(500);
        stubit.led.off();
        await stubit.wait(500);
      }
    }

  </script>
</body>

</html>
```

## Contribute

Build and 