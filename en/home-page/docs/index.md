---
template: templates/home-page.html
---

# WSO2 Enterprise Integrator Documentation

WSO2 Enterprise Integrator (WSO2 EI) 7.x is an open-source hybrid integration platform that enables API-centric integration using integration architecture styles such as microservices or centralized ESB. The platform provides a graphical drag-and-drop flow designer and a configuration-driven approach to build low-code integration solutions for cloud and container-native projects.

Enterprise Integrator 7.x series product suite consists of Micro Integrator and Streaming integrator runtimes.

<!--
 Adding temporary urls for navigation
-->
<h3>Get Started</h3>
<div style="display: flex; flex-wrap: wrap">
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/overview/quick-start-guide">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Quick Start Guide
                <div class="description" style="">
                Get up and running with WSO2 Enterprise Integrator in 5 minutes.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/overview/introduction">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Introduction
                <div class="description" style="">
                Introduces WSO2 Enterprise Integrator and describes what it can do.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/overview/key-concepts">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Key Concepts
                <div class="description" style="">
                The key concepts are for users who are new to WSO2 Enterprise Integrator and want to understand its functionality.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/develop/integration-development-kickstart">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Developing your First Integration
                <div class="description" style="">
                This helps you to build and run an integration scenario using WSO2 Integration Studio.
                </div>
            </div>
        </div>
        
    </a>
</div>
</div>

<h3>What can WSO2 Enterprise Integrator do?</h3>
<div style="display: flex; flex-wrap: wrap">
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/use-cases/integration-use-cases">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Routing and Transformation
                <div class="description" style="">
                Supports content-based routing, header-based routing, rules-based routing, and policy-based routing. Transforms the message to different formats.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/use-cases/integration-use-cases/#service-orchestration">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Restful Services and Orchestration
                <div class="description" style="">
                Exposes multiple fine-grained services using a single coarse-grained service.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="https://docs.wso2.com/display/IntegrationPatterns/">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Enterprise Messaging
                <div class="description" style="">
                Support for all enterprise integration patterns (EIPs) and common enterprise messaging scenarios.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/references/connectors/connectors-overview">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                SaaS Integration
                <div class="description" style="">
                Multiple connectors across categories such as payments, CRM, ERP, social networks, and legacy systems.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/overview/introduction">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Microservices Integration
                <div class="description" style="">
                Lightweight runtimes for container-based deployments. Native integration with container-management platforms.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/use-cases/integration-use-cases/#data-integration">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Data Integration
                <div class="description" style="">
                Decouples the data from the datasource layer and exposes them as data services.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="streaming-integrator/examples/tutorials-overview">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                ETL and Streaming Integration
                <div class="description" style="">
                Performs real-time estimates using data that is stored in files or stored in a database like MySQL.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/use-cases/integration-use-cases/#file-processing">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                File Integration
                <div class="description" style="">
                Supports automatic processing of files with large amounts of data.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="streaming-integrator/samples/CDCWithListeningMode">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                Change Data Capture (CDC)
                <div class="description" style="">
                Capture changes in data that occur in databases.
                </div>
            </div>
        </div>
        
    </a>
</div>
<div class="integratorDescription">
    <a style="color: #222222;" href="micro-integrator/references/connectors/connectors-overview">
        <div>
            <div>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 45 44.9" style="width: 60px;">
                    <path fill="#FF7300" d="M10.2 13h.7l.8 2 .1.4h.5l1.3.6.4.2.4-.1 2-.8.8 1-.7 1.8-.2.4.2.5.6 1.3.1.4.4.2 1.9.8a9.2 9.2 0 010 1.3l-1.9.8-.4.1-.1.5a7 7 0 01-.6 1.3l-.2.4.2.4.7 2-.9.8-1.9-.7-.4-.2-.4.2-1.3.6-.5.1-.1.4-.8 1.9a9.2 9.2 0 01-1.3 0l-.8-1.9-.2-.4-.4-.1a7 7 0 01-1.3-.6l-.5-.2-.4.2-1.9.7-.9-.9.8-1.9.1-.4-.2-.4-.5-1.3-.2-.5-.4-.1L1 23a9.2 9.2 0 010-1.3l1.8-.8.4-.2.2-.4.5-1.3.2-.5-.1-.4-.8-1.9 1-.9 1.8.8.4.1.5-.2 1.3-.5.4-.1.2-.5.8-1.8h.6m0-1H9l-1 2.4a8 8 0 00-1.5.6l-2.4-1c-.8.6-1.4 1.2-2 2l1 2.4a8 8 0 00-.6 1.5L0 21a10.3 10.3 0 000 2.7l2.4 1 .6 1.5-1 2.4c.6.7 1.2 1.3 2 1.9l2.4-1c.4.3 1 .5 1.5.6l1 2.4a10.3 10.3 0 002.7 0l1-2.4a8 8 0 001.5-.6l2.4 1c.7-.6 1.3-1.2 1.9-2l-1-2.3.6-1.5 2.4-1a10.3 10.3 0 000-2.7L18 20a8 8 0 00-.6-1.5l1-2.4c-.6-.8-1.2-1.4-2-2l-2.3 1a8 8 0 00-1.5-.6l-1-2.4h-1.4z"></path><path fill="#FF7300" d="M10.2 18.5a3.8 3.8 0 110 7.7 3.8 3.8 0 010-7.7m0-1a4.8 4.8 0 100 9.7 4.8 4.8 0 000-9.7z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M10.3 9.3v-6c0-1.5 1.3-2.8 2.9-2.8h21.5c1 0 2 .4 2.7 1.1l6 6a4 4 0 011.1 2.8v31a3 3 0 01-3 3H13.2a2.8 2.8 0 01-2.8-2.8V35"></path><ellipse fill="none" stroke="#434343" stroke-miterlimit="10" cx="28.7" cy="6.8" rx="4.1" ry="2.9"></ellipse><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M28.7 9.6v4.7M23.2 35.5h11v4.4h-11z"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M25 18.1l3.7-3.8 3.8 3.8-3.8 3.8z" stroke-width=".99999"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M23.9 26.4h9.6v4.7h-9.6zM28.7 21.9v4.5M28.7 31v4.5"></path><path fill="none" stroke="#434343" stroke-miterlimit="10" d="M32.6 18H38v19.7h-3.8"></path>
                </svg>
            </div>
            <div class="content" style="">
                B2B Integration
                <div class="description" style="">
                Supports integration that happens via popular B2B protocols.
                </div>
            </div>
        </div>
        
    </a>
</div>
</div>


<h3>Development Lifecycle</h3>
<div style="display:flex">
<div class="integratorDescription block">
    <a style="color: #222222" href="micro-integrator/setup/micro_integrator_deployment_overview">
        <div>
            <!-- <h3 class="home-title">Micro Integrator</h3> -->
            <div style="font-size: 1rem">
                Setup
            </div>
        </div>
    </a>
</div>
<div style="font-size: 1.8rem; padding: 20px 15px;"><i class="fw fw-right"></i></div>
<div class="integratorDescription block">
    <a style="color: #222222" href="micro-integrator/develop/intro-integration-development">
        <div>
            <!-- <h3 class="home-title">Micro Integrator</h3> -->
            <div style="font-size: 1rem">
                Develop
            </div>
        </div>
    </a>
</div>
<div style="font-size: 1.8rem; padding: 20px 15px;"><i class="fw fw-right"></i></div>
<div class="integratorDescription block">
    <a style="color: #222222" href="micro-integrator/setup/micro_integrator_deployment_overview">
        <div>
            <!-- <h3 class="home-title">Micro Integrator</h3> -->
            <div style="font-size: 1rem">
                Deploy
            </div>
        </div>
    </a>
</div>
<div style="font-size: 1.8rem; padding: 20px 15px;"><i class="fw fw-right"></i></div>
<div class="integratorDescription block">
    <a style="color: #222222" href="micro-integrator/overview/quick-start-guide">
        <div>
            <!-- <h3 class="home-title">Micro Integrator</h3> -->
            <div style="font-size: 1rem">
                Run
            </div>
        </div>
    </a>
</div>
<div style="font-size: 1.8rem; padding: 20px 15px;"><i class="fw fw-right"></i></div>
<div class="integratorDescription block">
    <a style="color: #222222" href="micro-integrator/administer-and-observe/">
        <div>
            <!-- <h3 class="home-title">Micro Integrator</h3> -->
            <div style="font-size: 1rem">
                Observe
            </div>
        </div>
    </a>
</div>
</div>
