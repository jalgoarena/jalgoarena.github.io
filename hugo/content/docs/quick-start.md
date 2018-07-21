+++
title = "Quick Start"
description = "Up and running in 10 minutes"
weight = 10
draft = false
toc = true
bref = "As a complete and highly performant algorithmic contest platform, JAlgoArena is here to help you get the most out of your own hosted contest. JAlgoArena comes with predefined problems (100+) to solve with different difficulty levels - software engineers with different skillset will be able to tackle problems meeting their skills. Starting up with JAlgoArena is fast and easy. Here's how to set up JAlgoArena, and what basic prerequisites are required."
+++

<h3 class="section-head" id="h-basic-template"><a href="#h-basic-template">Basic Template</a></h3>

<p>With Kube, you can set up your web framework and be on your way in under a minute. Just add this code to your web page for the basic template to take effect immediately.</p>

<pre class="code"><span class="hljs-meta">&lt;!DOCTYPE html&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">title</span>&gt;</span>Basic Template<span class="hljs-tag">&lt;/<span class="hljs-name">title</span>&gt;</span>

    <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">"utf-8"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"viewport"</span> <span class="hljs-attr">content</span>=<span class="hljs-string">"width=device-width, initial-scale=1"</span>&gt;</span>

    <span class="hljs-comment">&lt;!-- Kube CSS --&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">link</span> <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"assets/css/kube.css"</span>&gt;</span>

<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>Hello, world!<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span>

    <span class="hljs-comment">&lt;!-- Kube JS + jQuery are used for some functionality, but are not required for the basic setup --&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">"https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">"assets/js/kube.js"</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span></pre>


<h3 class="section-head" id="h-supported-browsers"><a href="#h-supported-browsers">Supported Browsers</a></h3>

<p>Kube supports the latest, stable releases of all major browsers:</p>
<ul>
    <li>Latest Chrome</li>
    <li>Latest Firefox</li>
    <li>Latest Safari</li>
    <li>Latest Opera</li>
    <li>Microsoft Edge</li>
    <li>Internet Explorer 11</li>
</ul>


<h3 class="section-head" id="h-development"><a href="#h-development">Development with Kube</a></h3>

<p>Kube has been designed to help you with web development, that's why it's so easy to use Kube when building websites. To move forward quickly and efficiently, just link <code>kube.scss</code> from Kube package: this file contains variables, mixins and everything you need to simplify daily routine tasks.
</p>

<p>
    For example, import kube.scss into your master.scss styles file, which you will later compile into <code>master.css</code>:
</p>

<pre class="code"><span class="hljs-comment">// master.scss</span>
@<span class="hljs-keyword">import</span> <span class="hljs-string">"dist/scss/kube.scss"</span>;</pre>

<p>
    Now all Kube's variables and mixins are readily available in <code>master.scss</code>,
    and you can use them whenever needed. For instance, here's how one of examples:
</p>

<pre class="code"><span class="hljs-comment">// master.scss</span>
<span class="hljs-keyword">@import</span> <span class="hljs-string">"dist/scss/kube.scss"</span>;

<span class="hljs-selector-id">#sidebar</span> {
    <span class="hljs-variable">@include</span> flex-item-width(<span class="hljs-number">200px</span>);
}</pre>

<p>Also, you could use settings from <code>variables.scss</code>:</p>


<pre class="code"><span class="hljs-comment">// master.scss</span>
@<span class="hljs-keyword">import</span> <span class="hljs-string">"dist/scss/kube.scss"</span>;

<span class="hljs-selector-id">#my-layout</span> {
    <span class="hljs-attribute">padding</span>: <span class="hljs-variable">$base-line</span>;
}</pre>
