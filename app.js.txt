const API_KEY = "AIzaSyD_vWDt2-OKmWdpJ8NXU-0nEqCFbZxEn1A";
const ROOT_FOLDER = "1mUHNXebrgRVpn3kefSMr8xIk_5WcekLm";
const PASSWORD = "linkdedebhai";

function unlock(){
  if(document.getElementById("passwordInput").value === PASSWORD){
    document.getElementById("lockScreen").style.display="none";
    document.getElementById("app").style.display="block";
    loadFolder(ROOT_FOLDER);
  }
}

async function loadFolder(folderId){
  let url = `https://www.googleapis.com/drive/v3/files?q='${folderId}'+in+parents&key=${API_KEY}&fields=files(id,name,mimeType)`;
  let res = await fetch(url);
  let data = await res.json();

  let browser = document.getElementById("browser");
  browser.innerHTML="";

  data.files.forEach(file=>{
    let div = document.createElement("div");
    div.className="item";

    if(file.mimeType.includes("folder")){
      div.innerText="📁 "+file.name;
      div.onclick=()=>loadFolder(file.id);
    } else if(file.mimeType.includes("video")){
      div.innerHTML = `<video controls src="https://www.googleapis.com/drive/v3/files/${file.id}?alt=media&key=${API_KEY}"></video>
                       <a href="https://www.googleapis.com/drive/v3/files/${file.id}?alt=media&key=${API_KEY}" download>Download</a>`;
    } else {
      div.innerText=file.name;
    }

    browser.appendChild(div);
  });
}
