#web_architectures 

![[Pasted image 20261008113208.png]]

local storage stuff

```javascript
"use strict";  
  
const notesKey = "notes";  
  
let notesData = [];  
  
let notesDiv = document.getElementById("notes");  
let contentArea = document.getElementById("content");  
  
document.querySelector("#saveBtn").addEventListener("click", saveData);  
  
loadData();  
  
function saveData()  
{  
    if (contentArea.value.trim().length === 0) return;  
  
    const entry =  
        {  
            date: new Date().toLocaleString("de-AT"),  
            content: contentArea.value.trim(),  
        };  
  
    notesData.push(entry);  
  
    contentArea.value = "";  
  
    renderData();  
  
    localStorage.setItem(notesKey, JSON.stringify(notesData));  
}  
  
function renderData()  
{  
    let output = "";  
  
    for (let entry of notesData)  
    {  
        output += `<p><em>${entry.date}</em> -- ${entry.content}</p>`;  
    }  
  
    notesDiv.innerHTML = output;  
}  
  
function loadData()  
{  
    notesData = JSON.parse(localStorage.getItem(notesKey)) ?? [];  
      
    renderData();  
}
```