# Live CSS Editor Bookmarklet

## Overview

The **Live CSS Editor** bookmarklet lets you edit the CSS of any webpage in real time. After selecting an element, you can modify its styles instantly and download the modified HTML of the element. It highlights elements on hover and displays their parent hierarchy.

## Features

- Select and edit any webpage element's CSS.
- Live style updates.
- View element hierarchy.
- Hover to highlight elements.
- Download modified HTML of the selected element.

## Installation

1. Copy the following code:

   ```javascript
   javascript: (function () {
     let selectedElement = null;
     let hoverOverlay = null;

     // Prompt to select an element
     alert("Click on an element to edit its CSS.");

     // CSS for the editor popup and highlight effect
     const editorStyle = `
       #cssEditorPopup {
         position: fixed;
         top: 10px;
         right: 10px;
         width: 360px;
         padding: 10px;
         background-color: white;
         border: 1px solid #ccc;
         box-shadow: 0 4px 8px rgba(0,0,0,0.1);
         z-index: 9999;
         font-family: Arial, sans-serif;
       }
       #cssEditorPopup textarea {
         width: 100%;
         height: 200px;
         padding: 5px;
         margin-bottom: 10px;
         border: 1px solid #ccc;
         border-radius: 4px;
         font-family: monospace;
       }
       #cssEditorPopup h3 {
         margin-top: 0;
         font-size: 16px;
         color: #333;
       }
       #cssEditorPopup p {
         font-size: 14px;
         margin-bottom: 8px;
         color: #666;
       }
       #cssEditorPopup button {
         padding: 5px 10px;
         margin-right: 5px;
         background-color: #4CAF50;
         color: white;
         border: none;
         border-radius: 3px;
         cursor: pointer;
       }
       #cssEditorPopup button:hover {
         background-color: #45a049;
       }
       #closeEditor {
         background-color: #f44336;
       }
       #closeEditor:hover {
         background-color: #d32f2f;
       }
       .hover-highlight {
         outline: 2px dashed blue !important;
         cursor: pointer;
       }
       #elementTree {
         font-family: monospace;
         font-size: 14px;
         color: #444;
         margin-bottom: 10px;
         background-color: #f9f9f9;
         padding: 5px;
         border: 1px solid #ddd;
         border-radius: 4px;
         max-height: 100px;
         overflow-y: auto;
       }
       #elementTree span {
         display: block;
         padding: 2px 0;
       }
       #elementTree .selected-element {
         font-weight: bold;
         color: #333;
       }
     `;

     // Inject CSS into the page
     const style = document.createElement("style");
     style.innerHTML = editorStyle;
     document.head.appendChild(style);

     // Create the popup editor
     const editorPopup = document.createElement("div");
     editorPopup.id = "cssEditorPopup";
     editorPopup.style.display = "none"; // Hidden initially
     editorPopup.innerHTML = `
       <h3>Edit CSS</h3>
       <p id="elementTag">Selected Element: None</p>
       <div id="elementTree">Element Hierarchy: None</div>
       <textarea id="cssEditorText"></textarea>
       <button id="applyCss">Apply CSS</button>
       <button id="saveHtml">Download Element</button>
       <button id="closeEditor">Close</button>
     `;
     document.body.appendChild(editorPopup);

     // Highlight elements on hover
     document.addEventListener("mouseover", function (event) {
       if (hoverOverlay) {
         hoverOverlay.classList.remove("hover-highlight");
       }
       event.target.classList.add("hover-highlight");
       hoverOverlay = event.target;
     });

     // Helper function to generate the element tree (parents)
     function getElementTree(element) {
       let elementTree = [];
       while (element) {
         let tagName = element.tagName.toLowerCase();
         if (element.id) {
           tagName += `#${element.id}`;
         }
         if (element.className) {
           tagName += `.${element.className.trim().split(/\s+/).join(".")}`;
         }
         elementTree.unshift(tagName);
         element = element.parentElement;
       }
       return elementTree;
     }

     // Function to display the element tree in the editor
     function displayElementTree(element) {
       const treeContainer = document.getElementById("elementTree");
       const elementTree = getElementTree(element);
       treeContainer.innerHTML = "";

       elementTree.forEach((elTag, index) => {
         const span = document.createElement("span");
         if (index === elementTree.length - 1) {
           span.classList.add("selected-element");
         }
         span.textContent = elTag;
         treeContainer.appendChild(span);
       });
     }

     // Event listener to select an element on click
     document.addEventListener(
       "click",
       function (event) {
         event.preventDefault();

         // Remove previous hover highlight
         if (hoverOverlay) {
           hoverOverlay.classList.remove("hover-highlight");
         }

         // Set the selected element
         selectedElement = event.target;
         const currentStyles = selectedElement.getAttribute("style") || "";
         document.getElementById("cssEditorText").value = currentStyles;

         // Show selected element's tag name
         document.getElementById(
           "elementTag"
         ).textContent = `Selected Element: ${selectedElement.tagName.toLowerCase()}`;

         // Show the element tree
         displayElementTree(selectedElement);

         // Show the editor popup
         editorPopup.style.display = "block";
       },
       { once: true }
     );

     // Apply the CSS when the "Apply CSS" button is clicked
     document.getElementById("applyCss").addEventListener("click", function () {
       const newCss = document.getElementById("cssEditorText").value;
       if (selectedElement) {
         selectedElement.setAttribute("style", newCss);
       }
     });

     // Download the modified HTML of the selected element when "Download Element" is clicked
     document.getElementById("saveHtml").addEventListener("click", function () {
       if (selectedElement) {
         const elementHtml = selectedElement.outerHTML;
         const blob = new Blob([elementHtml], { type: "text/html" });
         const downloadLink = document.createElement("a");
         downloadLink.href = URL.createObjectURL(blob);
         downloadLink.download = "modified_element.html";
         downloadLink.click();
       }
     });

     // Close the editor when the "Close" button is clicked
     document
       .getElementById("closeEditor")
       .addEventListener("click", function () {
         editorPopup.style.display = "none";
       });
   })();
   ```

## Usage

1. - Press ** CTRL+F12 to open console**.
2. - Paste above code in console and hit Enter.
3. Select an element by clicking on it.
4. Modify its styles in the editor that appears.
5. Apply changes or download the element's modified HTML.

## Example

- **Element Highlighting**: Hovering over an element will outline it with a border.
- **Live Editing**: Edit inline CSS styles in the popup and see immediate changes.
- **Download HTML**: After editing, download the selected element's updated HTML.

---

Enjoy making live CSS edits with ease!
