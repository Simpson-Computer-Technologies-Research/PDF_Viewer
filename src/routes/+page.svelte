
<div class="flex justify-center items-center">
    <div id="pdf-viewer"></div>
</div>

<script>
    import pdfjsWorker from "pdfjs-dist/build/pdf.worker.min.js?url";
    import * as pdfjs from 'pdfjs-dist';
    import { onMount } from 'svelte';

    // PDF Url
    const url = 'https://raw.githubusercontent.com/Simpson-Computer-Technologies-Research/LaTeX/176b2eeed26a8e53088c66079a1bdd141356f269/Physics%20Momentum%20%2B%20Energy/Momentum%20and%20Energy%20Big%20Book/build/main.pdf';

    // On site load
    onMount(async () => {
        const VIEWER_DOC = document.getElementById('pdf-viewer');

        // PDF.js
        pdfjs.GlobalWorkerOptions.workerSrc = pdfjsWorker;
        pdfjs.getDocument(url).promise.then((pdf) => {

            // Creating pages
            for (var i = 1; i < pdf.numPages + 1; i++) {
                pdf.getPage(i).then((page) => {
                    // Establish a new page viewport
                    var viewport = page.getViewport({
                        scale: Math.sqrt((window.innerWidth ** 2) + (window.innerHeight ** 2)) / 1920
                    });

                    // Creating a new canvas for each page
                    const CANVAS = document.createElement("canvas");    
                    CANVAS.className = 'border-2 border-black my-4 mx-4';         
                    CANVAS.height = viewport.height;
                    CANVAS.width = viewport.width;
                    VIEWER_DOC.appendChild(CANVAS);
                    
                    // Render the page
                    page.render({
                        canvasContext: CANVAS.getContext('2d'),
                        viewport: viewport
                    });
                });
            }
        });
    });
</script>
