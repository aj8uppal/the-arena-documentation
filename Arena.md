---


---

<h1 id="the-arena">The Arena</h1>
<ul>
<li><a href="#description">Description</a></li>
<li><a href="#frontend">Frontend</a></li>
<li><a href="#backend">Backend</a></li>
<li><a href="#database">Database</a></li>
<li><a href="#web-ops">Web Ops</a></li>
<li><a href="#tools">Tools</a></li>
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
|   |   ├── <b>config.js</b> (Modify endpoints/global vars here)
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
<h1 id="database">Database</h1>
<h2 id="interface">Interface</h2>
<h4 id="option-1-mongodb-compass">Option 1: MongoDB Compass</h4>
<ol>
<li>Download <a href="%5Bhttps://www.mongodb.com/products/compass%5D(https://www.mongodb.com/products/compass)">MongoDB Compass</a>.</li>
<li>Paste the following connection string (for user <code>db-access-user</code>, password <code>ArenaUser2357</code>):</li>
</ol>
<pre class=" language-json"><code class="prism  language-json">mongodb<span class="token operator">+</span>srv<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span>db<span class="token operator">-</span>access<span class="token operator">-</span>user<span class="token punctuation">:</span>ArenaUser2357@cluster0<span class="token punctuation">.</span>mayoh<span class="token punctuation">.</span>gcp<span class="token punctuation">.</span>mongodb<span class="token punctuation">.</span>net<span class="token operator">/</span>test<span class="token operator">?</span>authSource<span class="token operator">=</span>admin<span class="token operator">&amp;</span>replicaSet<span class="token operator">=</span>atlas<span class="token operator">-</span>6q0obm<span class="token operator">-</span>shard<span class="token number">-0</span><span class="token operator">&amp;</span>readPreference<span class="token operator">=</span>primary<span class="token operator">&amp;</span>appname<span class="token operator">=</span>MongoDB<span class="token operator">%</span>20Compass<span class="token operator">&amp;</span>ssl<span class="token operator">=</span><span class="token boolean">true</span>
</code></pre>
<ol start="3">
<li>Connect to DB</li>
</ol>
<h4 id="option-2-python-driver">Option 2: Python driver</h4>
<ol>
<li>Download <code>pymongo</code> with <code>pip install pymongo</code></li>
<li>Run the following code:</li>
</ol>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> pymongo <span class="token keyword">import</span> MongoClient
client <span class="token operator">=</span> pymongo<span class="token punctuation">.</span>MongoClient<span class="token punctuation">(</span><span class="token string">"mongodb+srv://db-access-user:Arena2357@cluster0.mayoh.gcp.mongodb.net/**&lt;dbname&gt;**?retryWrites=true&amp;w=majority"</span><span class="token punctuation">)</span>
</code></pre>
<p><sup>Note: make sure you replace <code>**dbname**</code> with the name of the designated <code>db</code>.</sup></p>
<h2 id="structure">Structure</h2>
<p>There are 3 tables in the DB: <a href="#programs"><code>programs</code></a>, <a href="#providers"><code>providers</code></a>, <a href="#jobs"><code>jobs</code></a>:</p>
<h3 id="programs"><code>programs</code></h3>

<table>
<thead>
<tr>
<th>field</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>id <strong>[required]</strong></td>
<td>integer</td>
<td><p align="center">Unique ID</p></td>
<td><p align="center">Unique ID of the program. Increments from 0.</p></td>
</tr>
<tr>
<td>title <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Program Title</p></td>
<td><p align="center">Full-length program title; ideally unique and easy to understand.</p></td>
</tr>
<tr>
<td>title_short <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Truncated Program Title</p></td>
<td><p align="center">Shortened title; only manually entered if full title exceeds 30 characters.</p></td>
</tr>
<tr>
<td>slug <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Program Slug</p></td>
<td><p align="center">Slugified title. See <a href="#slugify">tools/slugify</a> for more info.</p></td>
</tr>
<tr>
<td>provider <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Corresponding Provider of Program</p></td>
<td><p align="center">Restricted list; verified online, national providers. Must string match title in <a href="#providers"><code>providers</code></a> table.</p></td>
</tr>
<tr>
<td>type <strong>[required]</strong></td>
<td>array [string]</td>
<td><p align="center">Type of Program</p></td>
<td><p align="center">Restricted list; one of five core industries:</p><ul><li><code>"healthcare"</code></li><li><code>"manufacturing"</code></li><li><code>"education"</code></li><li><code>"business"</code></li><li><code>"IT"</code></li></ul><p></p></td>
</tr>
<tr>
<td>outcome <strong>[required]</strong></td>
<td>array [string]</td>
<td><p align="center">Outcome of Program</p></td>
<td><p align="center">Restricted list; one of four core signals:</p><ul><li><code>"degree"</code></li><li><code>"credential"</code></li><li><code>"certificate"</code></li><li><code>"skill"</code></li></ul><p></p></td>
</tr>
<tr>
<td>structure <strong>[required]</strong></td>
<td>array [string]</td>
<td><p align="center">Outcome of Program</p></td>
<td><p align="center">Program is <code>"scheduled"</code> if it has defined start or end date(s); otherwise <code>"self-paced"</code>.</p></td>
</tr>
<tr>
<td>url <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Program URL</p></td>
<td><p align="center">Program-specific URL; unique.</p></td>
</tr>
<tr>
<td>description <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Program Description</p></td>
<td><p align="center">Description; pulled from program URL</p></td>
</tr>
<tr>
<td>description_short <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Truncated Program Description</p></td>
<td><p align="center">Shortened title; only manually entered if full title exceeds 150 characters.</p></td>
</tr>
<tr>
<td>price_true <strong>[required]</strong></td>
<td>float</td>
<td><p align="center">Program Tuition/Cost</p></td>
<td><p align="center">Total cost; sometimes given, sometimes calculated based on cost per credit/semester.</p></td>
</tr>
<tr>
<td>rate_value <em>[optional]</em></td>
<td>float</td>
<td><p align="center">Program Rate (Value)</p></td>
<td><p align="center">Time variable; some programs communicate a rate of time (e.g. 10 hours per week); enter value here (e.g. 10, 20, etc.)</p></td>
</tr>
<tr>
<td>rate_unit <em>[optional]</em></td>
<td>string</td>
<td><p align="center">Program Rate (Unit)</p></td>
<td><p align="center">Time variable; some programs communicate a rate of time (e.g. 10 hours per week); enter unit here (e.g. hours per week)</p></td>
</tr>
<tr>
<td>duration_value <em>[optional]</em></td>
<td>float</td>
<td><p align="center">Program Duration (Value)</p></td>
<td><p align="center">Time variable; some programs communicate a duration of time (e.g. 10 weeks, 3 semesters, 120 unit hours, 80 credit hours, 30 clock hours, etc.); enter value here (e.g. 10, 20, etc.)</p></td>
</tr>
<tr>
<td>duration_unit <em>[optional]</em></td>
<td>string</td>
<td><p align="center">Program Duration (Unit)</p></td>
<td><p align="center">Time variable; some programs communicate a duration of time (e.g. 10 weeks, 3 semesters, 120 unit hours, 80 credit hours, 30 clock hours, etc.); enter unit here (e.g. credit hours, week, months, etc.)</p></td>
</tr>
<tr>
<td>financing <em>[optional]</em></td>
<td>array [string]</td>
<td><p align="center">Program Financing Option</p></td>
<td><p align="center">Restricted list:</p><ol><li><code>"public aid"</code> = federal / state grants or loans</li><li><code>"private aid"</code> = private grants or loans</li><li><code>"employer aid"</code> = tuition assistance</li><li><code>"financing"</code> = payment plans or loans; often determined at provider level</li></ol><p></p></td>
</tr>
<tr>
<td>credential <em>[optional]</em></td>
<td>array [string]</td>
<td><p align="center">Program Credential</p></td>
<td><p align="center">This field should be populated if and only if outcome= “credential”; please list relevant industry-recognized credentials earned as part of the completion of the program (typically an acronym)</p></td>
</tr>
</tbody>
</table><h3 id="providers"><code>providers</code></h3>

<table>
<thead>
<tr>
<th>field</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>id <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Unique ID</p></td>
<td><p align="center">Unique ID of the provider. Increments from 0, prefixed by <code>"t"</code>. E.g. <code>"t27"</code>.</p></td>
</tr>
<tr>
<td>title <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Provider Title</p></td>
<td><p align="center">Full-length provider title; <strong>must be unique</strong> and easy to understand.</p></td>
</tr>
<tr>
<td>img <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Provider Image URL</p></td>
<td><p align="center">Image URL for provider.</p></td>
</tr>
<tr>
<td>url <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Provider URL</p></td>
<td><p align="center">URL for provider.</p></td>
</tr>
</tbody>
</table><h3 id="jobs"><code>jobs</code></h3>

<table>
<thead>
<tr>
<th>field</th>
<th>type</th>
<th>description</th>
<th>long description</th>
</tr>
</thead>
<tbody>
<tr>
<td>id <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Unique ID</p></td>
<td><p align="center">Unique ID of the job. Increments from 0, prefixed by <code>"j"</code>. E.g. <code>"j27"</code>.</p></td>
</tr>
<tr>
<td>title <strong>[required]</strong></td>
<td>string</td>
<td><p align="center">Job Title</p></td>
<td><p align="center">Full-length job title; should be unique and easy to understand.</p></td>
</tr>
</tbody>
</table><h2 id="operations">Operations</h2>
<p><a href="%5Bhttps://pymongo.readthedocs.io/en/stable/%5D(https://pymongo.readthedocs.io/en/stable/)">Latest documentation for the MongoDB Python Driver, <code>pymongo</code>.</a></p>
<h3 id="examples">Examples</h3>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> pymongo <span class="token keyword">import</span> MongoClient
client <span class="token operator">=</span> pymongo<span class="token punctuation">.</span>MongoClient<span class="token punctuation">(</span><span class="token string">"mongodb+srv://db-access-user:Arena2357@cluster0.mayoh.gcp.mongodb.net"</span><span class="token punctuation">)</span>
data <span class="token operator">=</span> client<span class="token punctuation">.</span>data <span class="token comment">#access database</span>
programs <span class="token operator">=</span> data<span class="token punctuation">.</span>programs <span class="token comment">#access table</span>
</code></pre>
<h4 id="insert">Insert</h4>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> programs<span class="token punctuation">.</span>insert_one<span class="token punctuation">(</span><span class="token punctuation">{</span><span class="token string">"foo"</span><span class="token punctuation">:</span> <span class="token string">"bar"</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
<span class="token operator">&lt;</span>pymongo<span class="token punctuation">.</span>results<span class="token punctuation">.</span>InsertOneResult <span class="token builtin">object</span> at <span class="token number">0x1104be9b0</span><span class="token operator">&gt;</span>
</code></pre>
<h4 id="delete">Delete</h4>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> programs<span class="token punctuation">.</span>delete_one<span class="token punctuation">(</span><span class="token punctuation">{</span><span class="token string">"id"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
<span class="token operator">&lt;</span>pymongo<span class="token punctuation">.</span>results<span class="token punctuation">.</span>InsertOneResult <span class="token builtin">object</span> at <span class="token number">0x1104be9b0</span><span class="token operator">&gt;</span>
</code></pre>
<h4 id="find-one">Find One</h4>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> programs<span class="token punctuation">.</span>find_one<span class="token punctuation">(</span><span class="token punctuation">{</span><span class="token string">"id"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
<span class="token punctuation">{</span><span class="token string">'id'</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token string">'foo'</span><span class="token punctuation">:</span> <span class="token string">'bar'</span><span class="token punctuation">}</span>
</code></pre>
<h4 id="find-many-subpass--for-allsub">Find Many <sub>(pass <code>{}</code> for all)</sub></h4>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> programs<span class="token punctuation">.</span>find<span class="token punctuation">(</span><span class="token punctuation">{</span><span class="token string">"price_true"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span><span class="token string">"$gt"</span><span class="token punctuation">:</span> <span class="token number">4000</span><span class="token punctuation">}</span><span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token comment">#gets programs with price greater than $4,000</span>
<span class="token punctuation">[</span><span class="token punctuation">{</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span class="token punctuation">}</span><span class="token punctuation">]</span>
</code></pre>
<h4 id="updatesub">Update</h4>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> programs<span class="token punctuation">.</span>update_one<span class="token punctuation">(</span><span class="token punctuation">{</span><span class="token string">"id"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">}</span><span class="token punctuation">,</span> <span class="token punctuation">{</span><span class="token string">"$set"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span><span class="token string">"price_true"</span><span class="token punctuation">:</span> <span class="token number">10000</span><span class="token punctuation">}</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
<span class="token punctuation">[</span><span class="token punctuation">{</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span class="token punctuation">}</span><span class="token punctuation">]</span>
</code></pre>
<h1 id="web-ops">Web Ops</h1>
<h2 id="hosting-stack">Hosting Stack</h2>
<h4 id="database-mongodb-atlas"><a href="#database-hosting">Database</a>: MongoDB Atlas</h4>
<h4 id="full-stack-gcp-app-engine"><a href="#full-stack-hosting">Full-Stack</a>: GCP App Engine</h4>
<h2 id="database-hosting">Database Hosting</h2>
<p>The database is hosted through MongoDB Atlas. For information on interfacing, see the <a href="#database">database documentation</a>.</p>
<h4 id="cluster-management">Cluster Management:</h4>
<p><img src="https://i.imgur.com/sIy55Jv.png" alt="enter image description here"></p>
<h2 id="full-stack-hosting">Full-Stack Hosting</h2>
<p>The full stack application is a flask web app that is served through app engine. For information on building and deployment, see the <a href="#building-and-deploying-the-application">production deployment documentation</a>.</p>
<h4 id="app-engine-management">App Engine Management</h4>
<p><img src="https://i.imgur.com/yQPxvDJ.png" alt="enter image description here"></p>
<h1 id="tools">Tools</h1>
<h3 id="slugify"><code>slugify</code></h3>
<pre class=" language-python"><code class="prism  language-python">slugify <span class="token operator">=</span> <span class="token keyword">lambda</span> s<span class="token punctuation">:</span> <span class="token string">""</span><span class="token punctuation">.</span>join<span class="token punctuation">(</span><span class="token punctuation">[</span>char <span class="token keyword">for</span> char <span class="token keyword">in</span> s<span class="token punctuation">.</span>lower<span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>replace<span class="token punctuation">(</span><span class="token string">' '</span><span class="token punctuation">,</span>  <span class="token string">'-'</span><span class="token punctuation">)</span> <span class="token keyword">if</span> char<span class="token punctuation">.</span>isalpha<span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">or</span> char <span class="token operator">==</span>  <span class="token string">'-'</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
</code></pre>
<h3 id="shuffle"><a href="%5Bhttps://github.com/aj8uppal/arena-frontend/blob/master/src/Components/tools.js#L1%5D(https://github.com/aj8uppal/arena-frontend/blob/master/src/Components/tools.js#L1)"><code>shuffle</code></a></h3>
<h1 id="appendix">Appendix</h1>
<h2 id="adding-the-slider-back">Adding the slider back</h2>
<p>There are a few requirements for the slider component, once added back:</p>
<ul>
<li>Must export left and right parameters.</li>
<li>Must not include <code>&lt;input type="range"&gt;</code> since the start and end will be static.</li>
<li>Use the <code>&lt;EditableText /&gt;</code> component for the start and end ranges.</li>
<li>If recreating from the codebase, pay close attention to the code for the concentric circles on hover. There is a bug that prevents moving the slider again when you are hovering over the circles.</li>
</ul>

