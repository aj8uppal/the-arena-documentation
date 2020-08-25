---


---

<h1 id="the-arena">The Arena</h1>
<ul>
<li><a href="#description">Description</a></li>
<li><a href="#frontend">Frontend</a></li>
<li><a href="#backend">Backend</a></li>
<li><a href="#database">Database</a></li>
<li><a href="#web-ops">Web Ops</a></li>
</ul>
<h1 id="description">Description</h1>
<p>This repository hosts the documentation for the full stack environment of The Arena, Inc.</p>
<h1 id="frontend">Frontend</h1>
<pre><code>frontend
├── public (publicly accessible resources)
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
├── src (application source code)
|   ├── Components
|   |   ├── Source files for application
|   ├── App.css
|   ├── App.jsx
├── package.json (project metadata)
├── node_modules (project dependencies)
</code></pre>
<h3 id="npm-run-start"><code>npm run start</code></h3>
<p>Run the application in development mode. Open the application in <a href="http://localhost:3000/">http://localhost:3000/</a> to view it. Lazy-loading is implemented in dev, meaning that the react server does not need to be restarted when edits are made.</p>
<h3 id="npm-run-build"><code>npm run build</code></h3>
<p>Production grade build is created at <code>./build</code>. To run the production build, see <a href="#backend">backend</a>.</p>
<h4 id="note-upon-cloning-the-repository-run-npm-i-to-install-dependencies">Note: upon cloning the repository, run <code>npm i</code> to install dependencies</h4>
<h1 id="backend">Backend</h1>
<pre><code>backend
├── api.py (web application)
├── error.log (error log)
├── app.yaml (used for deployment)
├── requirements.txt (used for deployment)
├── build (production grade build created in frontend)
│   ├── ...
</code></pre>
<h2 id="dev-environment">Dev Environment</h2>
<h3 id="flask-run"><code>flask run</code></h3>
<p>Runs the back-end environment in development. Default port is 5000, which interfaces with the frontend. Lazy-loading is implemented in dev, meaning that the backend does not need to be restarted when edits are made.</p>
<p><sup>To change the proxy server on dev, navigate to <code>./frontend/package.json</code>, scroll to the bottom until you see <code>"proxy": "http://localhost:5000"</code>, and change to whatever port you are running the backend on.</sup></p>
<h2 id="production-environment">Production Environment</h2>
<h3 id="building-and-deploying-the-application">Building and deploying the application</h3>
<ol>
<li><a href="#npm-run-start">Build the react app</a></li>
<li>Copy the build directory from <code>frontend</code> to the <code>backend</code> directory (should be on the same directory level as <code>api.py</code>). Make sure you do not change the name of the directory from <code>build</code>, otherwise the web app will not be able to serve the static build.</li>
<li>Deploy the application to app engine:</li>
</ol>
<pre class=" language-bash"><code class="prism  language-bash">bash-3.2$ <span class="token function">pwd</span>
/Users/thearena/backend
bash-3.2$ <span class="token function">ls</span>
README.md  __pycache__  api.py  app.yaml  build error.log  requirements.txt
bash-3.2$ gcloud app deploy
Services to deploy:
descriptor:  <span class="token punctuation">[</span>/Users/thearena/backend/app.yaml<span class="token punctuation">]</span>
source:  <span class="token punctuation">[</span>/Users/thearena/backend<span class="token punctuation">]</span>
target project:  <span class="token punctuation">[</span>training-program-database<span class="token punctuation">]</span>
target service:  <span class="token punctuation">[</span>default<span class="token punctuation">]</span>
target version:  <span class="token punctuation">[</span>20200824t231953<span class="token punctuation">]</span>
target url:  <span class="token punctuation">[</span>https://training-program-database.uc.r.appspot.com<span class="token punctuation">]</span>

Do you want to <span class="token keyword">continue</span> <span class="token punctuation">(</span>Y/n<span class="token punctuation">)</span>?
</code></pre>
<p><strong>Hit Y and then let the deployment proceed. This make take anywhere from 10 minutes to an hour.</strong></p>
<h1 id="routes-api">Routes (<a href="#Backend">API</a>)</h1>
<p>This section details the API endpoints for the application</p>
<h2 id="search-for-program">Search For Program</h2>
<h4 id="post-apisearch_program"><code>POST /api/search_program</code></h4>
<p>Search the database based on a query string. Return search results</p>
<p><strong>Params:</strong></p>

<table>
<thead>
<tr>
<th>param</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>query</td>
<td>string  [required]</td>
<td>Search string</td>
<td><p align="center">The query is the search string that the DB is being queried for. Example: <code>"Bachelor's healthcare"</code>.</p></td>
</tr>
</tbody>
</table><p><code>curl -X POST -d '{"query": "bachelor healthcare"}' -H 'Content-Type: application/json' /api/search_program</code></p>
<p><strong>Note:</strong> This request uses <code>'Content-Type: application/json'</code>.</p>
<h2 id="query-programs">Query Programs</h2>
<h4 id="post-apiprograms"><code>POST /api/programs</code></h4>
<p>Identify programs based on designated field values.</p>
<p><strong>Params:</strong></p>

<table>
<thead>
<tr>
<th>param</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>field</td>
<td>array</td>
<td>Designated field</td>
<td><p align="center">The number of fields in the POST may vary. Each field corresponds to a database parameter.</p></td>
</tr>
</tbody>
</table><p>.</p>
<pre class=" language-json"><code class="prism  language-json">curl <span class="token operator">-</span>X POST <span class="token operator">-</span>d '
'<span class="token punctuation">{</span>
	<span class="token string">"outcome"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"skill"</span><span class="token punctuation">,</span> <span class="token string">"certificate"</span><span class="token punctuation">]</span><span class="token punctuation">,</span> 
	<span class="token string">"budget"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
		<span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span> 
		<span class="token string">"end"</span><span class="token punctuation">:</span> <span class="token number">5000</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>'
 <span class="token operator">-</span>H <span class="token string">'Content-Type: application/json'</span> <span class="token operator">/</span>api<span class="token operator">/</span>programs
</code></pre>
<p><strong>Note:</strong> The field <code>budget</code> accepts an object of the format shown above. This request uses <code>'Content-Type: application/json'</code>.</p>
<h2 id="get-program-by-id">Get Program (by id)</h2>
<h4 id="post-apiprogram"><code>POST /api/program</code></h4>
<p>Return program from database + related programs.</p>
<p><strong>Params:</strong></p>

<table>
<thead>
<tr>
<th>param</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>id</td>
<td>integer  [required]</td>
<td>Program ID</td>
<td><p align="center">Unique ID of the program</p></td>
</tr>
</tbody>
</table><p><code>curl -X POST -d '{"id": 258}' -H 'Content-Type: application/json' /api/search_program</code></p>
<p><strong>Note:</strong> This request uses <code>'Content-Type: application/json'</code>.</p>

