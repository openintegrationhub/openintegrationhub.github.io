---
layout: home
title: Introduction
nav_order: 1
permalink: /
---

<p align="center">
  <img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/large-oih-vertikal-zentriert.png" alt="Open Integration Hub" width="200"/>
</p>
<br>
<br>

# Welcome

Hi there! Glad you are here. This is the documentation of [Open Integration Hub](https://www.openintegrationhub.org/?lang=en), a free and open source ([Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)) Integration Framework.

You will find explanations of how it works, how to set it up and the idea behind each service. For the actual code, as well as more technical information head to [GitHub](https://github.com/openintegrationhub/openintegrationhub).

We are a community driven project. Please join us on [Slack](https://join.slack.com/t/openintegrationhub/shared_invite/zt-mba97vn9-xus3ZbVxnMr2oQwGegIk5Q) or open issues on [GitHub](https://github.com/openintegrationhub/openintegrationhub/issues) to give feedback and ask your questions. Also mind the [Contribution Guidelines](https://github.com/openintegrationhub/openintegrationhub/blob/master/CONTRIBUTING.md).

# What is Open Integration Hub?

The Open Integration Hub is a framework for easy data exchange between business applications. It provides a technical integration layer for your product or project. It is not meant as an enduser-ready application, but rather to give you a flexible set of services to enrich or build your own solution. You can easily create asynchronous [integration flows](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/FlowBasics.html) and benefit from a wide range of standardized open source [integration components](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/ComponentsBasics.html).

# Overview

<div class="oih-docs-overview-container">
    <a class="item" href="https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/Intro.html">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#0A0B09"><path d="M10 21.71c.988 0 3.532.28 5 .91v-15.61c-.805-.53-3.571-1.01-5-1.01-1.849 0-4.131.31-5 1v15.56c1.656-.8 4.378-.85 5-.85zm-5.5 2.29l-.186-.04c-.189-.07-.314-.26-.314-.46v-16.72c0-.13.048-.25.137-.34 1.304-1.37 5.113-1.44 5.863-1.44 1.237 0 4.914.44 5.862 1.44.089.09.138.21.138.34v16.72c0 .2-.125.39-.315.46-.189.08-.406.03-.548-.12-.554-.58-3.609-1.13-5.137-1.13-1.957 0-4.4.35-5.138 1.13-.097.1-.228.16-.362.16zM21 21.71c.988 0 3.532.28 5 .91v-15.61c-.805-.53-3.571-1.01-5-1.01-1.849 0-4.131.31-5 1v15.56c1.656-.8 4.378-.85 5-.85zm-5.5 2.29l-.186-.04c-.189-.07-.314-.26-.314-.46v-16.72c0-.13.048-.25.137-.34 1.304-1.37 5.113-1.44 5.863-1.44 1.237 0 4.914.44 5.862 1.44.089.09.138.21.138.34v16.72c0 .2-.125.39-.315.46-.189.08-.405.03-.548-.12-.554-.58-3.609-1.13-5.137-1.13-1.957 0-4.4.35-5.138 1.13-.097.1-.228.16-.362.16zM29.5 26l-.043-.01-11.093-.96c-.347.62-1.349.97-2.864.97-1.516 0-2.518-.35-2.865-.97l-11.092.96c-.129.02-.278-.03-.381-.13-.104-.09-.162-.22-.162-.36v-17.51c0-.24.169-.44.402-.49l2.5-.49c.271-.06.534.12.588.39.054.27-.122.53-.393.59l-2.097.41v16.55l10.957-.95c.131-.01.277.03.381.13.103.09.162.23.162.37 0 .17.587.5 2 .5s2-.33 2-.5c0-.14.058-.28.162-.37.103-.1.25-.14.381-.13l10.957.95v-16.55l-2.098-.41c-.271-.06-.446-.32-.393-.59.054-.27.324-.45.588-.39l2.5.49c.234.05.403.25.403.49v17.51c0 .14-.059.27-.162.36-.093.09-.213.14-.338.14M13 25c-.277 0-.5-.23-.5-.5v-1.5c0-.28.223-.5.5-.5.276 0 .5.22.5.5v1.5c0 .27-.224.5-.5.5m5 0c-.277 0-.5-.23-.5-.5v-2c0-.28.223-.5.5-.5.276 0 .5.22.5.5v2c0 .27-.224.5-.5.5"/></g></svg>
        <span class="title">Basic Concepts</span>
        <span class="description">Learn about Components, Flows and Transformations </span>
    </a>
    <a class="item" href="https://openintegrationhub.github.io/docs/2%20-%20SystemArchitecture/Intro.html">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#000"><path d="M15.5 16c-.28 0-.5-.22-.5-.5v-4c0-.27.22-.5.5-.5s.5.23.5.5v4c0 .28-.22.5-.5.5"/><path d="M24.5 20c-.28 0-.5-.22-.5-.5v-1.5c0-1.1-.9-2-2-2h-13c-1.1 0-2 .9-2 2v1.5c0 .28-.22.5-.5.5s-.5-.22-.5-.5v-1.5c0-1.65 1.35-3 3-3h13c1.65 0 3 1.35 3 3v1.5c0 .28-.22.5-.5.5m-9-18c-2.48 0-4.5 2.02-4.5 4.5s2.02 4.5 4.5 4.5 4.5-2.02 4.5-4.5-2.02-4.5-4.5-4.5zm0 10c-3.03 0-5.5-2.46-5.5-5.5 0-3.03 2.47-5.5 5.5-5.5s5.5 2.47 5.5 5.5c0 3.04-2.47 5.5-5.5 5.5zM6.5 20c-2.48 0-4.5 2.02-4.5 4.5s2.02 4.5 4.5 4.5 4.5-2.02 4.5-4.5-2.02-4.5-4.5-4.5zm0 10c-3.03 0-5.5-2.46-5.5-5.5 0-3.03 2.47-5.5 5.5-5.5s5.5 2.47 5.5 5.5c0 3.04-2.47 5.5-5.5 5.5zm18-10c-2.48 0-4.5 2.02-4.5 4.5s2.02 4.5 4.5 4.5 4.5-2.02 4.5-4.5-2.02-4.5-4.5-4.5zm0 10c-3.03 0-5.5-2.46-5.5-5.5 0-3.03 2.47-5.5 5.5-5.5s5.5 2.47 5.5 5.5c0 3.04-2.47 5.5-5.5 5.5z"/></g></svg>
            <span class="title">System Architecture</span>
            <span class="description">How it works</span>
    </a>
  <a class="item" href="https://openintegrationhub.github.io/docs/3%20-%20GettingStarted/Intro.html">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#0A0B09"><path d="M16.737 19.984c.131 0 .258.06.353.15l8.237 8.24c.4.4.954.63 1.52.63h.003c1.185 0 2.15-.97 2.15-2.15 0-.58-.224-1.12-.63-1.53l-6.756-6.75c-.942-.94-1.44-2.03-1.611-3.52l-.019-.16c-.117-1.05-.238-2.13-1.394-3.26-1.08-1.07-2.339-1.61-3.53-1.63-.895.01-1.76.26-2.535.73l3.482 2.09c.15.09.243.25.243.43 0 1.73-.963 3.29-2.513 4.06l-.264.13c-.154.08-.334.07-.481-.01l-2.966-1.79c.111 1.12.584 2.16 1.369 3 .979 1.04 2.303 1.61 3.73 1.61.452 0 .919-.08 1.468-.24l.144-.03zm10.113 10.02h-.003c-.841 0-1.632-.33-2.227-.93l-8.03-8.03c-.525.14-.997.21-1.465.21-1.68 0-3.305-.7-4.457-1.93-1.169-1.24-1.758-2.86-1.658-4.57.011-.18.116-.34.276-.42.153-.07.359-.08.507.01l3.479 2.09.018-.01c1.126-.57 1.854-1.66 1.949-2.9l-3.887-2.33c-.14-.08-.23-.23-.242-.39-.011-.17.058-.32.186-.43 1.092-.88 2.39-1.36 3.751-1.37 1.455-.03 2.973.66 4.246 1.91 1.407 1.39 1.561 2.76 1.685 3.86l.018.17c.144 1.25.553 2.15 1.325 2.92l6.756 6.76c.595.59.923 1.38.923 2.22 0 1.74-1.413 3.16-3.15 3.16zM10.625 30.004c-.206 0-.417-.06-.611-.16l-5.029-2.99c-.524-.3-.752-.94-.53-1.52l1.353-3.46c-.502-.72-.901-1.42-1.21-2.14l-3.562-.52c-.587-.08-1.036-.61-1.036-1.23v-5.96c0-.63.449-1.15 1.046-1.23l3.552-.52c.319-.75.724-1.46 1.209-2.14l-1.351-3.46c-.208-.59.027-1.24.54-1.53l5.007-2.97c.55-.31 1.209-.18 1.58.3l2.22 2.92c.9-.1 1.494-.1 2.393 0l2.219-2.91c.379-.5 1.06-.63 1.586-.31l5.012 2.98c.525.29.753.94.532 1.52l-1.353 3.46c.502.72.899 1.42 1.209 2.14l3.563.52c.586.08 1.036.6 1.036 1.23v5.96c0 .62-.45 1.15-1.045 1.23l-2.455.36c-.272.03-.528-.15-.568-.42-.04-.28.149-.53.422-.57l2.464-.36c.105-.02.182-.12.182-.24v-5.96c0-.12-.076-.22-.172-.24l-3.853-.56c-.177-.03-.325-.15-.391-.31-.323-.81-.767-1.6-1.361-2.41-.1-.14-.124-.32-.062-.48l1.45-3.71c.043-.11.002-.24-.097-.29l-5.029-2.99c-.109-.07-.219-.02-.275.05l-2.391 3.15c-.109.14-.285.21-.462.19-1.067-.14-1.647-.14-2.715 0-.175.02-.353-.05-.461-.19l-2.393-3.15c-.065-.08-.183-.1-.286-.04l-5.006 2.97c-.111.06-.139.21-.102.31l1.444 3.7c.063.16.038.34-.065.48-.566.76-1.022 1.57-1.357 2.41-.066.16-.215.28-.391.31l-3.843.56c-.107.02-.182.12-.182.24v5.96c0 .12.074.22.173.24l3.851.56c.177.03.326.15.392.31.322.81.767 1.6 1.36 2.41.101.14.124.32.062.48l-1.45 3.71c-.043.11-.002.24.097.29l5.028 2.99c.093.05.211.03.277-.05l2.391-3.15c.108-.14.287-.22.461-.19.774.1 1.942.1 2.715 0 .176-.03.353.05.462.19l2.392 3.15c.065.08.185.1.285.04l1.441-.85c.237-.14.544-.07.685.17.141.24.063.55-.175.69l-1.451.86c-.55.31-1.21.18-1.579-.3l-2.221-2.92c-.692.07-1.7.07-2.393 0l-2.219 2.91c-.237.31-.593.48-.959.48"/></g></svg>
            <span class="title">Deployment</span>
            <span class="description">A quickstart for your own setup</span>
    </a>
<a class="item" href="https://openintegrationhub.github.io/docs/4%20-%20ForDevelopers/Intro.html">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#000"><path d="M10.364 23c-.18 0-.352-.101-.439-.276l-1.958-4c-.069-.141-.069-.307 0-.448l1.958-4c.121-.247.415-.346.657-.223.242.123.34.424.219.671l-1.848 3.776 1.848 3.776c.12.247.023.548-.219.671-.07.036-.145.053-.218.053m2.447 2l-.155-.026c-.256-.087-.395-.37-.309-.632l3.916-12c.085-.262.36-.405.619-.316.256.087.395.37.309.632l-3.916 12c-.069.21-.259.342-.464.342m6.363-2c-.073 0-.148-.017-.218-.053-.242-.123-.34-.424-.219-.671l1.848-3.776-1.848-3.776c-.12-.247-.023-.548.219-.671s.535-.024.657.223l1.958 4c.069.141.069.307 0 .448l-1.958 4c-.086.175-.258.276-.439.276"/><path d="M4.979 29h19.579v-28h-12.034l-7.545 7.707v20.293zm20.068 1h-20.558c-.27 0-.489-.224-.489-.5v-21c0-.133.052-.26.143-.354l7.832-8c.092-.093.216-.146.347-.146h12.726c.27 0 .489.224.489.5v29c0 .276-.219.5-.489.5zM12.321 9h-7.832c-.27 0-.489-.224-.489-.5s.219-.5.489-.5h7.342v-7.5c0-.276.219-.5.489-.5s.489.224.489.5v8c0 .276-.219.5-.489.5"/></g></svg>
        <span class="title">For Developers</span>
        <span class="description">More technical guidelines</span>
    </a>
<a class="item" href="https://openintegrationhub.github.io/docs/5%20-%20Services/Services.html">
        <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#0A0B09"><path d="M5.97 1c-1.09 0-1.97.88-1.97 1.97v24.06c0 1.08.88 1.97 1.97 1.97h18.06c1.09 0 1.97-.89 1.97-1.97v-24.06c0-1.09-.88-1.97-1.97-1.97h-18.06zm18.06 29h-18.06c-1.64 0-2.97-1.34-2.97-2.97v-24.06c0-1.64 1.33-2.97 2.97-2.97h18.06c1.64 0 2.97 1.33 2.97 2.97v24.06c0 1.63-1.33 2.97-2.97 2.97zM7.5 30c-.28 0-.5-.23-.5-.5v-29c0-.28.22-.5.5-.5s.5.22.5.5v29c0 .27-.22.5-.5.5m9.5-24c-3.31 0-6 2.69-6 6s2.69 6 6 6 6-2.69 6-6-2.69-6-6-6zm0 13c-3.86 0-7-3.14-7-7s3.14-7 7-7 7 3.14 7 7-3.14 7-7 7zM17 6c-.84 0-2 2.28-2 6 0 3.71 1.16 6 2 6 .83 0 2-2.29 2-6 0-3.72-1.17-6-2-6zm0 13c-1.95 0-3-3.61-3-7 0-3.4 1.05-7 3-7s3 3.6 3 7c0 3.39-1.05 7-3 7zM23 10h-12c-.28 0-.5-.23-.5-.5 0-.28.22-.5.5-.5h12c.28 0 .5.22.5.5 0 .27-.22.5-.5.5m0 5h-12c-.28 0-.5-.23-.5-.5 0-.28.22-.5.5-.5h12c.28 0 .5.22.5.5 0 .27-.22.5-.5.5m.5 7h-13c-.28 0-.5-.23-.5-.5 0-.28.22-.5.5-.5h13c.28 0 .5.22.5.5 0 .27-.22.5-.5.5m0 3h-13c-.28 0-.5-.23-.5-.5 0-.28.22-.5.5-.5h13c.28 0 .5.22.5.5 0 .27-.22.5-.5.5"/></g></svg>
        <span class="title">Services</span>
        <span class="description">An overview of all the services</span>
    </a>
    <a class="item" href="https://openintegrationhub.github.io/docs/6%20-%20ReferenceDocumentation/Intro.html">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#0A0B09"><path d="M24.5 6h-12c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h12c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0 5h-12c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h12c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0 5h-12c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h12c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0 5h-12c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h12c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0 5h-12c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h12c.276 0 .5.22.5.5 0 .27-.224.5-.5.5"/><path d="M4.755 1c-.417 0-.755.34-.755.75v26.49c0 .42.338.76.755.76h22.489c.417 0 .756-.34.756-.76v-26.49c0-.41-.339-.75-.756-.75h-22.489zm22.489 29h-22.489c-.967 0-1.755-.79-1.755-1.76v-26.49c0-.97.788-1.75 1.755-1.75h22.489c.968 0 1.756.78 1.756 1.75v26.49c0 .97-.788 1.76-1.756 1.76zM6.5 19h-4c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h4c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0 6h-4c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h4c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0-12h-4c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h4c.276 0 .5.22.5.5 0 .27-.224.5-.5.5m0-6h-4c-.277 0-.5-.23-.5-.5 0-.28.223-.5.5-.5h4c.276 0 .5.22.5.5 0 .27-.224.5-.5.5"/><path d="M7.5 6c-.276 0-.5.22-.5.5 0 .27.224.5.5.5.275 0 .5-.23.5-.5 0-.28-.225-.5-.5-.5zm0 2c-.828 0-1.5-.68-1.5-1.5 0-.83.672-1.5 1.5-1.5.827 0 1.5.67 1.5 1.5 0 .82-.673 1.5-1.5 1.5zm0 4c-.276 0-.5.22-.5.5 0 .27.224.5.5.5.275 0 .5-.23.5-.5 0-.28-.225-.5-.5-.5zm0 2c-.828 0-1.5-.68-1.5-1.5 0-.83.672-1.5 1.5-1.5.827 0 1.5.67 1.5 1.5 0 .82-.673 1.5-1.5 1.5zm0 4c-.276 0-.5.22-.5.5 0 .27.224.5.5.5.275 0 .5-.23.5-.5 0-.28-.225-.5-.5-.5zm0 2c-.828 0-1.5-.68-1.5-1.5 0-.83.672-1.5 1.5-1.5.827 0 1.5.67 1.5 1.5 0 .82-.673 1.5-1.5 1.5zm0 4c-.276 0-.5.22-.5.5 0 .27.224.5.5.5.275 0 .5-.23.5-.5 0-.28-.225-.5-.5-.5zm0 2c-.828 0-1.5-.68-1.5-1.5 0-.83.672-1.5 1.5-1.5.827 0 1.5.67 1.5 1.5 0 .82-.673 1.5-1.5 1.5z"/></g></svg>
        <span class="title">Reference Documentation</span>
        <span class="description">API and Data Model Reference</span>
    </a>
    <a class="item" href="mailto:info@cloudecosystem.org">
    <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30"><g fill="#000">
        <path d="M15.5,13"/>
        <path d="M12,24C5.383,24,0,18.617,0,12S5.383,0,12,0s12,5.383,12,12c0,2.862-2.376,5-4.5,5c-2.24,0-4.5-1.237-4.5-4l1.008-5.589
          c0.049-0.272,0.309-0.452,0.581-0.402c0.272,0.049,0.452,0.31,0.402,0.581l-1,5.5C16,15.214,17.885,16,19.5,16
          c1.619,0,3.5-1.747,3.5-4c0-6.065-4.935-11-11-11S1,5.935,1,12s4.935,11,11,11c2.513,0,4.878-0.825,6.84-2.385
          c0.216-0.173,0.53-0.137,0.702,0.08c0.172,0.216,0.136,0.53-0.08,0.702C17.322,23.1,14.741,24,12,24z"/>
        <path d="M11.5,17c-1.305,0-2.511-0.624-3.308-1.712c-0.837-1.142-1.087-2.614-0.668-3.938c0.673-2.133,2.224-3.648,4.255-4.159
          c1.733-0.438,3.529-0.063,4.571,0.952c0.198,0.192,0.202,0.509,0.009,0.707c-0.192,0.199-0.508,0.201-0.707,0.009
          c-0.785-0.765-2.241-1.045-3.628-0.698c-0.991,0.25-2.77,1.034-3.546,3.49c-0.322,1.021-0.127,2.16,0.522,3.046
          C9.605,15.525,10.518,16,11.5,16c2.043,0,3.221-1.862,3.513-3.114c0.063-0.269,0.333-0.437,0.601-0.373
          c0.269,0.063,0.436,0.332,0.373,0.601C15.535,15.045,13.849,17,11.5,17z"/></g></svg>
        <span class="title">Get in touch!</span>
        <span class="description">Give us your questions and feedback</span>
    </a>

</div>
