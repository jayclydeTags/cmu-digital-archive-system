<template>
    <div style="position: relative;">
        <!-- Alert Message -->
        <v-overlay :value="isLoading">
            <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>

        <div class="wrapper" v-if="getFileType === 'pdf'">
            <v-container>
                <v-btn id="prev" color="success">
                    <v-icon>mdi-skip-previous</v-icon>
                </v-btn>
                <v-btn id="next" color="success">
                    <v-icon>mdi-skip-next</v-icon>
                </v-btn>
                &nbsp; &nbsp;
                <span>Page: <span id="page_num"></span> / <span id="page_count"></span></span>


            </v-container>
            <canvas id="the-canvas" class="img-fluid img-thumbnail"></canvas>


            <!-- <canvas id="the-canvas" class="img-fluid"></canvas> -->

            <div class="textLayer"></div>

            <!-- <object :data="getFile.filecontent" type="application/pdf">
                <iframe :src="`https://docs.google.com/viewer?url=${getFile.filecontent}&embedded=true#toolbar=0`"></iframe>
             </object> -->


            <!-- <embed :src="getFile.filecontent + '#toolbar=0'" type="application/pdf">
            <div class="embed-cover"></div> -->


            <!-- <div id="pdf">
                <object width="100%" height="650" type="application/pdf"
                    :data="getFile.filecontent + `#zoom=85&scrollbar=0&toolbar=0&navpanes=0`" id="pdf_content"
                    style="pointer-events: none;">
                    <p>Insert your error message here, if the PDF cannot be displayed.</p>
                </object>
            </div> -->

        </div>
        <div class="wrapper" v-else>
            <!-- <v-container>
                <v-btn v-show="request.document_type === 'Public'" id="next" color="error" @click="downloadDocuments">
                    <v-icon>mdi-download</v-icon>
                </v-btn>
            </v-container> -->
            <!-- <img class="img-fluid" :src="getFile.filecontent" /> -->
            <v-container>
                <!-- <v-card class="blue-grey pa-4 pa-md-2 ">
                    <img class="image--size"  />
                </v-card> -->
                <div class="text-center">
                    <img :src="getFile.filecontent" class="image--size rounded img-fluid img-thumbnail">
                </div>
            </v-container>

        </div>
    </div>
</template>
<script>
import AlertComponent from "./../../../AlertComponent.vue";

export default {
    props: ["docs", "request"],
    components: {
        AlertComponent
    },
    data() {
        return {
            file: null,
            msgStatus: false,
        };
    },
    computed: {
        getFile() {
            return this.docs;
        },
        getFileType() {
            return this.docs.filetype;
        },
        //ISLOADING COMPUTED
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading;
            },
            set: function (newVal) {
                return newVal;
            },
        },
    },

    methods: {
        downloadDocuments() {
            this.msgStatus = true;
            this.isLoading = true;
            let data = {
                file_url: this.docs.filecontent,
                file_name: this.request.filename,
            };
            this.$store.dispatch("downloadDocuments", data);
        },
        setPdvViewer() {
            if (this.getFileType === "pdf") {

                var url = this.getFile.filecontent;

                // Loaded via <script> tag, create shortcut to access PDF.js exports.
                var pdfjsLib = window["pdfjs-dist/build/pdf"];

                // The workerSrc property shall be specified.
                pdfjsLib.GlobalWorkerOptions.workerSrc = "//mozilla.github.io/pdf.js/build/pdf.worker.js";

                var pdfDoc = null,
                    pageNum = 1,
                    pageRendering = false,
                    pageNumPending = null,
                    //scale = 0.8,
                    scale = 1.5,
                    canvas = document.getElementById("the-canvas"),
                    textLayer = document.querySelector(".textLayer"),
                    container = document.querySelector(".wrapper"),
                    ctx = canvas.getContext("2d");

                /**
                 * Get page info from document, resize canvas accordingly, and render page.
                 * @param num Page number.
                 */
                function renderPage(num) {
                    pageRendering = true;
                    // Using promise to fetch the page
                    pdfDoc.getPage(num).then(function (page) {
                        var viewport = page.getViewport({ scale: scale });
                        // scale = container.clientWidth / viewport.width;
                        // viewport = page.getViewport(scale);
                        canvas.height = viewport.height;
                        canvas.width = viewport.width;

                        // var displayWidth = 1;
                        // canvas.style.width = `${(viewport.width * displayWidth) / scale}px`;
                        // canvas.style.height = `${(viewport.height * displayWidth) / scale}px`;
                        // textLayer.style.width = `${(viewport.width * displayWidth) / scale}px`;
                        // textLayer.style.height = `${(viewport.height * displayWidth) / scale}px`;

                        // Render PDF page into canvas context
                        var renderContext = {
                            canvasContext: ctx,
                            viewport: viewport,
                        };
                        var renderTask = page.render(renderContext);

                        // Wait for rendering to finish
                        renderTask.promise.then(function () {
                            pageRendering = false;
                            if (pageNumPending !== null) {
                                // New page rendering is pending
                                renderPage(pageNumPending);
                                pageNumPending = null;
                            }
                        })
                            .then(function () {
                                // Returns a promise, on resolving it will return text contents of the page
                                return page.getTextContent();
                            })
                            .then(function (textContent) {
                                // Assign CSS to the textLayer elemen

                                textLayer.style.left = canvas.offsetLeft + "px";
                                textLayer.style.top = canvas.offsetTop + "px";
                                textLayer.style.height = canvas.offsetHeight + "px";
                                textLayer.style.width = canvas.offsetWidth + "px";

                                // Pass the data to the method for rendering of text over the pdf canvas.
                                pdfjsLib.renderTextLayer({
                                    textContent: textContent,
                                    container: textLayer,
                                    viewport: viewport,
                                    textDivs: [],
                                    transform: [resolution, 0, 0, resolution, 0, 0] // force it bigger siz
                                });
                            });
                    });

                    // Update page counters
                    document.getElementById("page_num").textContent = num;
                }

                /**
                 * If another page rendering in progress, waits until the rendering is
                 * finised. Otherwise, executes rendering immediately.
                 */
                function queueRenderPage(num) {
                    if (pageRendering) {
                        pageNumPending = num;
                    } else {
                        renderPage(num);
                    }
                }

                /**
                 * Displays previous page.
                 */
                function onPrevPage() {
                    if (pageNum <= 1) {
                        return;
                    }
                    pageNum--;
                    queueRenderPage(pageNum);
                }
                document.getElementById("prev").addEventListener("click", onPrevPage);

                /**
                 * Displays next page.
                 */
                function onNextPage() {
                    if (pageNum >= pdfDoc.numPages) {
                        return;
                    }
                    pageNum++;
                    queueRenderPage(pageNum);
                }
                document.getElementById("next").addEventListener("click", onNextPage);

                /**
                 * Asynchronously downloads PDF.
                 */
                pdfjsLib.getDocument(url).promise.then(function (pdfDoc_) {
                    pdfDoc = pdfDoc_;
                    document.getElementById("page_count").textContent = pdfDoc.numPages;

                    // Initial/first page rendering
                    renderPage(pageNum);
                });
            }

        },
    },
    mounted() {
        document.addEventListener("contextmenu", (event) =>
            event.preventDefault()
        );
        this.setPdvViewer();
    },
};
</script>
<style scoped>
#the-canvas {
    direction: ltr;
}

/* .img-fluid {
    max-width: 100%;
    height: auto;
} */

embed {
    width: 100%;
    height: 90vh;
}

object {
    width: 100%;
    height: 90vh;
}

.embed-cover {
    position: relative;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;

    /* margin-right: 20px; */

    /* Just for demonstration, remove this part */
    /* opacity: 0.25;
    background-color: transparent; */
}

.wrapper {
    position: relative;
    overflow: hidden;
}


.image--size {
    width: 100%;
    height: 100%;
}


@media screen and (min-width: 600px) {
    .image--size {
        width: 50%;
    }
}
</style>
