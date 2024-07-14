<div class="container">
    <div class="sidebar left">
        <input
            type="text"
            bind:value={presentationName}
            class="presentation-name"
            placeholder="Presentation Name"
        />
        {#each slides as slide}
            <div class="slide-item">
                <input
                    type="text"
                    bind:value={slide.name}
                    on:input={(e) => changeSlideName(slide.id, e)}
                    on:click={() => selectSlide(slide.id)}
                    class:active={currentSlideId === slide.id}
                />
            </div>
        {/each}
        <button on:click={addSlide}>Add Slide</button>

        <div class="side-tools left">
            <button on:click={savePresentation}>Save</button>
            <button on:click={shareViaEmail}>Share</button>
            <button on:click={downloadPresentation}>Download</button>
        </div>
    </div>

    <div class="main">
        <div class="toolbar">
            <label for="font-select">Font:</label>
            <select id="font-select" on:change={changeFont}>
                <option value="Arial">Arial</option>
                <option value="Courier New">Courier New</option>
                <option value="Georgia">Georgia</option>
                <option value="Times New Roman">Times New Roman</option>
                <option value="Verdana">Verdana</option>
            </select>
            <label for="color-picker">Color:</label>
            <input type="color" id="color-picker" on:change={changeColor} />
            <label for="font-size">Font Size:</label>
            <input type="number" id="font-size" on:input={changeFontSize} min="8" max="72" />
            <button on:click={addText}>Add Text</button>
        </div>

        <div class="slide-editor">
            {#each slides as slide (slide.id)}
                {#if slide.id === currentSlideId}
                    <div
                        class="slide-content"
                        contenteditable="true"
                        style="font-size: {slide.content.title.fontSize}; font-family: {slide.content.title.fontFamily}; color: {slide.content.title.color}; top: 20px;"
                        on:input={(e) => handleContentEditableInput(e, 'title')}
                        on:dragstart={handleDragStart}
                        on:drag={handleDrag}
                        on:dragend={handleDragEnd}
                        draggable="true"
                        data-field="title"
                    >
                        {slide.content.title.text}
                    </div>
                    <div
                        class="slide-content"
                        contenteditable="true"
                        style="font-size: {slide.content.subtitle.fontSize}; font-family: {slide.content.subtitle.fontFamily}; color: {slide.content.subtitle.color}; top: 80px;"
                        on:input={(e) => handleContentEditableInput(e, 'subtitle')}
                        on:dragstart={handleDragStart}
                        on:drag={handleDrag}
                        on:dragend={handleDragEnd}
                        draggable="true"
                        data-field="subtitle"
                    >
                        {slide.content.subtitle.text}
                    </div>
                    <div
                        class="slide-content"
                        contenteditable="true"
                        style="font-size: {slide.content.contentFontSize}; font-family: {slide.content.contentFontFamily}; color: {slide.content.contentColor}; top: 140px;"
                        on:input={(e) => handleContentEditableInput(e, 'content')}
                        on:dragstart={handleDragStart}
                        on:drag={handleDrag}
                        on:dragend={handleDragEnd}
                        draggable="true"
                        data-field="content"
                    >
                        {@html slide.content.content}
                    </div>
                {/if}
            {/each}
        </div>
    </div>
</div>

<style>
    /* Global styles for the entire component */
    .container {
        display: flex;
        height: 100vh;
        background-color: #f4f4f4;
    }

    .sidebar {
        width: 200px;
        background-color: #333;
        color: #fff;
        padding: 10px;
        box-sizing: border-box;
    }

    .sidebar.left {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: flex-start;
    }

    .main {
        flex: 1;
        display: flex;
        flex-direction: column;
    }

    .toolbar {
        display: flex;
        align-items: center;
        padding: 10px;
        background-color: #fff;
        border-bottom: 1px solid #ccc;
    }

    .toolbar select,
    .toolbar input[type="color"],
    .toolbar input[type="number"] {
        margin-left: 10px;
    }

    .slide-editor {
        flex: 1;
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 20px;
        background-color: #fff;
        overflow: hidden; /* Ensures content does not overflow */
    }

    .slide-content {
        position: absolute;
        border: 1px solid #ccc;
        padding: 10px;
        background-color: #fff;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        cursor: move; /* Set cursor to indicate draggable */
    }

    .side-tools {
        display: flex;
        flex-direction: column;
        padding: 10px;
        background-color: #fff;
        border-left: 1px solid #ccc;
    }

    .side-tools.left {
        align-items: flex-start;
        margin-top: 20px;
        border-top: 1px solid #ccc;
        padding-top: 10px;
    }

    .side-tools.left button {
        margin-bottom: 10px;
        color: #333; /* Darken the text color for visibility */
        background-color: #f4f4f4; /* Subtle color background */
        border: none;
        padding: 10px 15px;
        cursor: pointer;
    }

    .side-tools.left button:hover {
        background-color: #ccc; /* Darken background on hover */
    }

    .side-tools.left button:active {
        transform: translateY(1px);
    }

    .side-tools button {
        margin-left: 10px;
        color: #333; /* Darken the text color for visibility */
        background-color: #f4f4f4; /* Subtle color background */
        border: none;
        padding: 10px 15px;
        cursor: pointer;
    }

    .side-tools button:hover {
        background-color: #ccc; /* Darken background on hover */
    }

    .side-tools button:active {
        transform: translateY(1px);
    }

    .slide-item input {
        color: #000; /* Ensure the text color is black for visibility */
        background-color: #fff; /* Ensure background is white for contrast */
        border: none;
        width: 100%;
        padding: 5px;
        box-sizing: border-box;
    }

    .slide-item input:focus {
        outline: none;
    }

    /* Ensure presentation name input is visible */
    .presentation-name {
        color: #000; /* Black text */
        background-color: #fff; /* White background */
        border: none;
        padding: 5px;
        margin-bottom: 10px;
        width: calc(100% - 20px); /* Adjust width as needed */
        box-sizing: border-box;
    }
</style>

<script>
    import { onMount } from 'svelte';
    import { createEventDispatcher } from 'svelte';
    import jsPDF from 'jspdf';

    let presentationName = '';
    let slides = [{
        id: 1,
        name: 'Slide 1',
        content: {
            title: { text: 'Title', fontSize: '36px', fontFamily: 'Arial', color: '#000' },
            subtitle: { text: 'Subtitle', fontSize: '24px', fontFamily: 'Arial', color: '#000' },
            content: '<p contenteditable="true">Content goes here. Click to edit.</p>'
        }
    }];
    let currentSlideId = 1;
    let activeElement = null;

    const dispatch = createEventDispatcher();

    function addSlide() {
        const newSlideId = slides.length + 1;
        slides = [...slides, {
            id: newSlideId,
            name: `Slide ${newSlideId}`,
            content: {
                title: { text: 'Title', fontSize: '36px', fontFamily: 'Arial', color: '#000' },
                subtitle: { text: 'Subtitle', fontSize: '24px', fontFamily: 'Arial', color: '#000' },
                content: '<p contenteditable="true">Content goes here. Click to edit.</p>'
            }
        }];
        selectSlide(newSlideId);
    }

    function selectSlide(id) {
        currentSlideId = id;
    }

    function addText() {
        const slideIndex = slides.findIndex(slide => slide.id === currentSlideId);
        slides[slideIndex].content.content += '<p contenteditable="true">New Text</p>';
    }

    function changeSlideName(id, event) {
        const slideIndex = slides.findIndex(slide => slide.id === id);
        slides[slideIndex].name = event.target.value;
    }

    function changeFont(event) {
        if (activeElement) {
            const fontFamily = event.target.value;
            activeElement.style.fontFamily = fontFamily;

            updateSlideContent(activeElement);
        }
    }

    function changeColor(event) {
        if (activeElement) {
            const color = event.target.value;
            activeElement.style.color = color;

            updateSlideContent(activeElement);
        }
    }

    function changeFontSize(event) {
        if (activeElement) {
            const fontSize = event.target.value + 'px';
            activeElement.style.fontSize = fontSize;

            updateSlideContent(activeElement);
        }
    }

    function savePresentation() {
        alert(`Saving presentation "${presentationName}"`);
        // Implement actual saving logic here
    }

    function shareViaEmail() {
        const subject = encodeURIComponent(`Check out "${presentationName}"`);
        const body = encodeURIComponent(`Hey there! I thought you might be interested in this presentation: ${window.location.href}`);
        window.location.href = `mailto:?subject=${subject}&body=${body}`;
    }

    function downloadPresentation() {
    let htmlContent = '<html><head><title>' + presentationName + '</title></head><body>';

    slides.forEach((slide, index) => {
        htmlContent += `<div style="font-size: ${slide.content.title.fontSize}; font-family: ${slide.content.title.fontFamily}; color: ${slide.content.title.color};">${slide.content.title.text}</div>`;
        htmlContent += `<div style="font-size: ${slide.content.subtitle.fontSize}; font-family: ${slide.content.subtitle.fontFamily}; color: ${slide.content.subtitle.color};">${slide.content.subtitle.text}</div>`;
        htmlContent += slide.content.content;
    });

    htmlContent += '</body></html>';

    const blob = new Blob([htmlContent], { type: 'text/html' });
    const url = URL.createObjectURL(blob);

    const link = document.createElement('a');
    link.href = url;
    link.download = `${presentationName}.html`;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
}


    function handleContentEditableInput(event, field) {
        const slideIndex = slides.findIndex(slide => slide.id === currentSlideId);
        slides[slideIndex].content[field].text = event.target.innerText;
    }

    // Variables to handle drag-and-drop functionality
    let offsetX = 0;
    let offsetY = 0;

    function handleDragStart(event) {
        const { left, top } = event.target.getBoundingClientRect();
        offsetX = event.clientX - left;
        offsetY = event.clientY - top;

        activeElement = event.target;
        event.dataTransfer.setDragImage(new Image(), 0, 0); // Hide default drag image
    }

    function handleDrag(event) {
        if (activeElement) {
            const slideEditor = document.querySelector('.slide-editor');
            const editorRect = slideEditor.getBoundingClientRect();

            const newX = event.clientX - offsetX - editorRect.left;
            const newY = event.clientY - offsetY - editorRect.top;

            // Ensure the element stays within the bounds of the white screen
            if (newX >= 0 && newX + activeElement.offsetWidth <= slideEditor.clientWidth) {
                activeElement.style.left = `${newX}px`;
            }
            if (newY >= 0 && newY + activeElement.offsetHeight <= slideEditor.clientHeight) {
                activeElement.style.top = `${newY}px`;
            }
        }
    }

    function handleDragEnd() {
        if (activeElement) {
            updateSlideContent(activeElement);
            activeElement = null;
        }
    }

    function updateSlideContent(element) {
        const slideIndex = slides.findIndex(slide => slide.id === currentSlideId);
        const field = element.dataset.field;
        slides[slideIndex].content[field].fontSize = element.style.fontSize;
        slides[slideIndex].content[field].fontFamily = element.style.fontFamily;
        slides[slideIndex].content[field].color = element.style.color;
    }
</script>
