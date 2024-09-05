<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Example Use WsUploadPicture</title>
  </head>
  <body>
    <form id="upload" enctype="multipart/form-data">
      <input type="file" name="file" id="file" accept="image/*" required>
      <button type="submit">Confirm</button>
    </form>
    <div id="result"></div>
    <script>
      document.getElementById("upload").addEventListener("submit", async function(e) {
        e.preventDefault();
        let t = document.getElementById("file");
        if (!t.files.length) {
          document.getElementById("result").innerText = "Please select a file.";
          return
        }
        let l = new FormData;
        l.append("file", t.files[0]);
        try {
          let n = await fetch("WsUploadPicture.php", {
              method: "POST",
              body: l
            }),
            s = await n.json();
          s.status === 'success' ? document.getElementById("result").innerHTML = `
            <a href="${s.url}" target="_blank">
	        	<img src="${s.url}">
			</a>` : document.getElementById("result").innerText = `Upload failed: ${s.msg}`
        } catch (r) {
          document.getElementById("result").innerText = `Error: ${r.message}`
        }
      });
    </script>
  </body>
</html>
