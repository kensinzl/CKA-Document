---


---

<h1 id="solution-architect-associate">Solution Architect Associate</h1>
<ul>
<li><a href="https://www.aws.training/">AWS Cloud Practitioner Essentials</a></li>
<li><a href="https://aws.amazon.com/certification/certification-prep/?cta=saa_examprep">https://aws.amazon.com/certification/certification-prep/?cta=saa_examprep</a></li>
<li><a href="https://instant.1point3acres.com/thread/690305">https://instant.1point3acres.com/thread/690305</a></li>
</ul>
<h3 id="ec2-elastic-computing-cloud">EC2 <em>Elastic Computing Cloud</em></h3>
<ul>
<li>Providing the sucure, resizable compute capacity on the Amzon Cloud</li>
<li>Just pay what you have used</li>
<li>The EC2 instance has public and private IP,  public IP is for the external to access, and every time to start-stop instance, the public IP will change but the private IP is not changed.</li>
</ul>
<h3 id="ec2-instance-type">EC2 <em>Instance Type</em></h3>
<ul>
<li>General purpose instance: Balance compute, memory and networking</li>
<li>Compute optimized instance: high-performance computing</li>
<li>Memory optimized instance: big dataset</li>
<li>Accelerated computing instance: graphic and streaming</li>
<li>Storage optimized instance: local dataset</li>
</ul>
<h3 id="ec2-price">EC2 <em>Price</em></h3>
<ul>
<li>On-Demand: pay what you used, eg: test for the workload</li>
<li>Save plan</li>
<li>Reserved instance: already know the predicatble usage</li>
<li>Spot instance: will use up to 90% on-demand price and need to torelate the interuption</li>
</ul>
<h3 id="elb-elastic-loader-balance">ELB <em>Elastic Loader Balance</em></h3>
<ul>
<li>automaticallt distribute the incoming request across several service, eg: EC2</li>
<li>Front - ELB - Back, decoupling architect</li>
<li>ELB can auto increase/decrease the amount of service based on the demand of request</li>
<li>It is a regional level because the EC instance may be allocated on different AZ</li>
</ul>
<h3 id="region">Region</h3>
<ul>
<li>Compliance</li>
<li>Proximity</li>
<li>Feature Availability</li>
<li>Price</li>
<li>Each region is made up of multiple data centers</li>
<li>Regions are geographically isolated areas</li>
<li>Regions contain availability zones</li>
</ul>
<h3 id="availability-zone-az">Availability Zone <em>AZ</em></h3>
<ul>
<li>AWS call a single data center or a group of data centers</li>
<li>Data Center with redundant power, networking and etc</li>
<li>Launghing a EC instance like a virtual machine on a physical hardware installed among AZ</li>
<li>A large scal disacter may strike a whole AZ</li>
<li>So even your app is one alone instance in one particular AZ, but better to set up multiple instances across different AZ in a Region, put them as far apart as you can</li>
</ul>
<h3 id="edge-location">Edge Location</h3>
<ul>
<li>Edge location run AWS CloudFront to help get content closer to your clients no matter where they are in the world</li>
</ul>
<blockquote>
<p>Suppose that your company’s data is in the South Africa, but your vital client is living in China, so from the AWS Region Proximity, the request speed is slow.<br>
You have no necessary to copy all data from South Africa into China.<br>
Instead of requiring the client to get their data from South Aferica into China, you can cache a copy locally at an edge location which is close your cutomers in China.<br>
When a client in China requests one of your files, AWS CloudFront retrives the files from the cache in the edge location and deliever it for this client. The data delievery to client is faster because the comes from the edge location near China instead of the original source in South Aferica.</p>
</blockquote>
<h3 id="block-level">Block level</h3>
<ul>
<li>block-level storage is for the EC instance which is ephemeral like K8S pods, so if the instance is terminated then the storage is also gone</li>
</ul>
<h3 id="elastic-block-store-ebs">Elastic Block Store <em>EBS</em></h3>
<ul>
<li>block-level storage volumes for the EC instances, even terminate the instance, the storage is still there, this the default behavior, of course, you can set from console to delete when EC2 instance is terminated.</li>
</ul>
<h3 id="ebs-vs-s3simple-storage-service">EBS Vs S3(Simple Storage Service)</h3>
<blockquote>
<p>If your storage object is huge like 1G</p>
</blockquote>
<ul>
<li>S3 is the object storage, so even a tiny change but still overwrite the whole 1G, because the object is a whole thing. Plus one benefit, S3 have the URL to retrieve for the stuff.</li>
<li>EBS is the block storage, so the blocks are split, then the change is only reflected on the particular scene</li>
<li>If you occasionally change the big stuff, then the EBS is the better</li>
<li>If you need a complex change, then S3 is the better</li>
</ul>
<h3 id="ebs-vs-efs">EBS Vs EFS</h3>
<ul>
<li>EBS attaches to EC2 instance(one EBS only to one EC2 instance, but one EC2 may has multiple EBS, like PVC - EBS), and EBS is on the AZ level</li>
<li>EBS and EC2 instance should be in the same AZ</li>
<li>EBS does not automatically scale</li>
<li>We can make a snapshot of the EBS and restore it into a different AZ</li>
<li>EFS can has multiple instacnes cross AZ reading and writting from it at the same time</li>
<li>EFS store the data across the multiple AZ and be a automatically scalable system</li>
</ul>
<h3 id="security-group">Security Group</h3>
<ul>
<li>I think SG is similar with the NetworkPolicy of K8S, both are the separate firewall of the server</li>
<li>So SG to instances is 1 to N, eg: NetworkPolicy to multiple Pods</li>
<li>The running instance can edit the SG from the management console</li>
</ul>
<h3 id="how-to-enter-the-instance">How to enter the Instance</h3>
<ul>
<li>ssh -i [Your Public Key] ec2-user@[Your Instance Public IP], SSH from Terminal</li>
<li>EC2 - Selection one Instance - Connect, SSH from the Admin Console</li>
</ul>
<h3 id="elastic-ips">Elastic IPs</h3>
<ul>
<li>When stop and start an EC instance, its public IP will be changed</li>
<li>If you need a fixed public IP for your instance, you need an Elastic IP</li>
<li>try to avoid using the Elastic IPs 1) poor architectual decisions 2) using a random public IP and register a DNS name 3) using LoadBalance and not use a public IP</li>
</ul>
<h3 id="vertical-and-horizontal-scalability">vertical and horizontal scalability</h3>
<ul>
<li>vertical scalability means increasing the size of the instance, eg: junior to senior</li>
<li>horizontal scalability means increasing the number of instances/system for your application</li>
</ul>
<h3 id="load-balancer">Load Balancer</h3>
<ul>
<li>I think this can cross the AZ, eg: classic load balancer to three AZ instances</li>
<li>Application Load Balance: works similar K8S Ingress, eg: /user =&gt; direct into User instance, /food =&gt; direct into Food instance</li>
<li>Sticky Session: make some users’ requests stick into one particular server, it with cookies among header for server to distinguish</li>
</ul>
<h3 id="connection-drain">Connection Drain</h3>
<ul>
<li>Gracefully shutdown the instance, if the request has been already trafficed into Instance A, then will continue to execute until finish or time-out, then Instance A will be moved from ELB.</li>
<li>At the same time, the new request will never be trafficed into Instance A, will going to Instance B and C.</li>
</ul>
<h3 id="rds-read-replicas">RDS read replicas</h3>
<ul>
<li>scale the read DB replicas not the main DB, up to 5 DB</li>
<li>within AZ, across AZ or across Region</li>
<li>main DB has the read &amp; write action, but the replicas DBs are async from the main DB, that means the read are eventually consistent</li>
<li>In AWS, there is a network cost when data is from one AZ to another, but the exception is RDS read replicas in the same Region, it is for free</li>
<li>RDS Multi AZ diaster recovery DB is sync replication, one DNS name, just for the failover and not used on the usual time</li>
<li>RDS- from single AZ to multi -&gt; snapshot the DB and restore it into another AZ and then auto create asyn between these two DB</li>
</ul>
<h3 id="s3---buckets">S3 - Buckets</h3>
<ul>
<li>Amazon S3 allows people to store objects(file) in the buckets(directories)</li>
<li>Buckets must have a globally unique name</li>
<li>Buckets even is a global service, but is a region level</li>
<li>Objects(file) have a Key which the FULL path</li>
</ul>
<p>Hi! I’m your first Markdown file in <strong>StackEdit</strong>. If you want to learn about StackEdit, you can read me. If you want to play with Markdown, you can edit me. Once you have finished with me, you can create new files by opening the <strong>file explorer</strong> on the left corner of the navigation bar.</p>
<h1 id="files">Files</h1>
<p>StackEdit stores your files in your browser, which means all your files are automatically saved locally and are accessible <strong>offline!</strong></p>
<h2 id="create-files-and-folders">Create files and folders</h2>
<p>The file explorer is accessible using the button in left corner of the navigation bar. You can create a new file by clicking the <strong>New file</strong> button in the file explorer. You can also create folders by clicking the <strong>New folder</strong> button.</p>
<h2 id="switch-to-another-file">Switch to another file</h2>
<p>All your files and folders are presented as a tree in the file explorer. You can switch from one to another by clicking a file in the tree.</p>
<h2 id="rename-a-file">Rename a file</h2>
<p>You can rename the current file by clicking the file name in the navigation bar or by clicking the <strong>Rename</strong> button in the file explorer.</p>
<h2 id="delete-a-file">Delete a file</h2>
<p>You can delete the current file by clicking the <strong>Remove</strong> button in the file explorer. The file will be moved into the <strong>Trash</strong> folder and automatically deleted after 7 days of inactivity.</p>
<h2 id="export-a-file">Export a file</h2>
<p>You can export the current file by clicking <strong>Export to disk</strong> in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.</p>
<h1 id="synchronization">Synchronization</h1>
<p>Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your <strong>Google Drive</strong>, your <strong>Dropbox</strong> and your <strong>GitHub</strong> accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow… The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.</p>
<p>There are two types of synchronization and they can complement each other:</p>
<ul>
<li>
<p>The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.</p>
<blockquote>
<p>To start syncing your workspace, just sign in with Google in the menu.</p>
</blockquote>
</li>
<li>
<p>The file synchronization will keep one file of the workspace synced with one or multiple files in <strong>Google Drive</strong>, <strong>Dropbox</strong> or <strong>GitHub</strong>.</p>
<blockquote>
<p>Before starting to sync files, you must link an account in the <strong>Synchronize</strong> sub-menu.</p>
</blockquote>
</li>
</ul>
<h2 id="open-a-file">Open a file</h2>
<p>You can open a file from <strong>Google Drive</strong>, <strong>Dropbox</strong> or <strong>GitHub</strong> by opening the <strong>Synchronize</strong> sub-menu and clicking <strong>Open from</strong>. Once opened in the workspace, any modification in the file will be automatically synced.</p>
<h2 id="save-a-file">Save a file</h2>
<p>You can save any file of the workspace to <strong>Google Drive</strong>, <strong>Dropbox</strong> or <strong>GitHub</strong> by opening the <strong>Synchronize</strong> sub-menu and clicking <strong>Save on</strong>. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.</p>
<h2 id="synchronize-a-file">Synchronize a file</h2>
<p>Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.</p>
<p>If you just have modified your file and you want to force syncing, click the <strong>Synchronize now</strong> button in the navigation bar.</p>
<blockquote>
<p><strong>Note:</strong> The <strong>Synchronize now</strong> button is disabled if you have no file to synchronize.</p>
</blockquote>
<h2 id="manage-file-synchronization">Manage file synchronization</h2>
<p>Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking <strong>File synchronization</strong> in the <strong>Synchronize</strong> sub-menu. This allows you to list and remove synchronized locations that are linked to your file.</p>
<h1 id="publication">Publication</h1>
<p>Publishing in StackEdit makes it simple for you to publish online your files. Once you’re happy with a file, you can publish it to different hosting platforms like <strong>Blogger</strong>, <strong>Dropbox</strong>, <strong>Gist</strong>, <strong>GitHub</strong>, <strong>Google Drive</strong>, <strong>WordPress</strong> and <strong>Zendesk</strong>. With <a href="http://handlebarsjs.com/">Handlebars templates</a>, you have full control over what you export.</p>
<blockquote>
<p>Before starting to publish, you must link an account in the <strong>Publish</strong> sub-menu.</p>
</blockquote>
<h2 id="publish-a-file">Publish a File</h2>
<p>You can publish your file by opening the <strong>Publish</strong> sub-menu and by clicking <strong>Publish to</strong>. For some locations, you can choose between the following formats:</p>
<ul>
<li>Markdown: publish the Markdown text on a website that can interpret it (<strong>GitHub</strong> for instance),</li>
<li>HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).</li>
</ul>
<h2 id="update-a-publication">Update a publication</h2>
<p>After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the <strong>Publish now</strong> button in the navigation bar.</p>
<blockquote>
<p><strong>Note:</strong> The <strong>Publish now</strong> button is disabled if your file has not been published yet.</p>
</blockquote>
<h2 id="manage-file-publication">Manage file publication</h2>
<p>Since one file can be published to multiple locations, you can list and manage publish locations by clicking <strong>File publication</strong> in the <strong>Publish</strong> sub-menu. This allows you to list and remove publication locations that are linked to your file.</p>
<h1 id="markdown-extensions">Markdown extensions</h1>
<p>StackEdit extends the standard Markdown syntax by adding extra <strong>Markdown extensions</strong>, providing you with some nice features.</p>
<blockquote>
<p><strong>ProTip:</strong> You can disable any <strong>Markdown extension</strong> in the <strong>File properties</strong> dialog.</p>
</blockquote>
<h2 id="smartypants">SmartyPants</h2>
<p>SmartyPants converts ASCII punctuation characters into “smart” typographic punctuation HTML entities. For example:</p>

<table>
<thead>
<tr>
<th></th>
<th>ASCII</th>
<th>HTML</th>
</tr>
</thead>
<tbody>
<tr>
<td>Single backticks</td>
<td><code>'Isn't this fun?'</code></td>
<td>‘Isn’t this fun?’</td>
</tr>
<tr>
<td>Quotes</td>
<td><code>"Isn't this fun?"</code></td>
<td>“Isn’t this fun?”</td>
</tr>
<tr>
<td>Dashes</td>
<td><code>-- is en-dash, --- is em-dash</code></td>
<td>– is en-dash, — is em-dash</td>
</tr>
</tbody>
</table><h2 id="katex">KaTeX</h2>
<p>You can render LaTeX mathematical expressions using <a href="https://khan.github.io/KaTeX/">KaTeX</a>:</p>
<p>The <em>Gamma function</em> satisfying <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi mathvariant="normal">Γ</mi><mo stretchy="false">(</mo><mi>n</mi><mo stretchy="false">)</mo><mo>=</mo><mo stretchy="false">(</mo><mi>n</mi><mo>−</mo><mn>1</mn><mo stretchy="false">)</mo><mo stretchy="false">!</mo><mspace width="1em"></mspace><mi mathvariant="normal">∀</mi><mi>n</mi><mo>∈</mo><mi mathvariant="double-struck">N</mi></mrow><annotation encoding="application/x-tex">\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mord">Γ</span><span class="mopen">(</span><span class="mord mathnormal">n</span><span class="mclose">)</span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right: 0.277778em;"></span></span><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mopen">(</span><span class="mord mathnormal">n</span><span class="mspace" style="margin-right: 0.222222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right: 0.222222em;"></span></span><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mord">1</span><span class="mclose">)!</span><span class="mspace" style="margin-right: 1em;"></span><span class="mord">∀</span><span class="mord mathnormal">n</span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">∈</span><span class="mspace" style="margin-right: 0.277778em;"></span></span><span class="base"><span class="strut" style="height: 0.68889em; vertical-align: 0em;"></span><span class="mord mathbb">N</span></span></span></span></span> is via the Euler integral</p>
<p><span class="katex--display"><span class="katex-display"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><semantics><mrow><mi mathvariant="normal">Γ</mi><mo stretchy="false">(</mo><mi>z</mi><mo stretchy="false">)</mo><mo>=</mo><msubsup><mo>∫</mo><mn>0</mn><mi mathvariant="normal">∞</mi></msubsup><msup><mi>t</mi><mrow><mi>z</mi><mo>−</mo><mn>1</mn></mrow></msup><msup><mi>e</mi><mrow><mo>−</mo><mi>t</mi></mrow></msup><mi>d</mi><mi>t</mi> <mi mathvariant="normal">.</mi></mrow><annotation encoding="application/x-tex">
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mord">Γ</span><span class="mopen">(</span><span class="mord mathnormal" style="margin-right: 0.04398em;">z</span><span class="mclose">)</span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right: 0.277778em;"></span></span><span class="base"><span class="strut" style="height: 2.32624em; vertical-align: -0.91195em;"></span><span class="mop"><span class="mop op-symbol large-op" style="margin-right: 0.44445em; position: relative; top: -0.001125em;">∫</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.41429em;"><span class="" style="top: -1.78805em; margin-left: -0.44445em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight">0</span></span></span><span class="" style="top: -3.8129em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight">∞</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.91195em;"><span class=""></span></span></span></span></span></span><span class="mspace" style="margin-right: 0.166667em;"></span><span class="mord"><span class="mord mathnormal">t</span><span class="msupsub"><span class="vlist-t"><span class="vlist-r"><span class="vlist" style="height: 0.864108em;"><span class="" style="top: -3.113em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mathnormal mtight" style="margin-right: 0.04398em;">z</span><span class="mbin mtight">−</span><span class="mord mtight">1</span></span></span></span></span></span></span></span></span><span class="mord"><span class="mord mathnormal">e</span><span class="msupsub"><span class="vlist-t"><span class="vlist-r"><span class="vlist" style="height: 0.843556em;"><span class="" style="top: -3.113em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mtight">−</span><span class="mord mathnormal mtight">t</span></span></span></span></span></span></span></span></span><span class="mord mathnormal">d</span><span class="mord mathnormal">t</span><span class="mspace" style="margin-right: 0.166667em;"></span><span class="mord">.</span></span></span></span></span></span></p>
<blockquote>
<p>You can find more information about <strong>LaTeX</strong> mathematical expressions <a href="http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference">here</a>.</p>
</blockquote>
<h2 id="uml-diagrams">UML diagrams</h2>
<p>You can render UML diagrams using <a href="https://mermaidjs.github.io/">Mermaid</a>. For example, this will produce a sequence diagram:</p>
<pre class=" language-mermaid"><svg id="mermaid-svg-Lwf1aBTS8qpsa97g" width="100%" xmlns="http://www.w3.org/2000/svg" height="543" style="max-width: 814px;" viewBox="-50 -10 814 543"><style>#mermaid-svg-Lwf1aBTS8qpsa97g{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#000000;}#mermaid-svg-Lwf1aBTS8qpsa97g .error-icon{fill:#552222;}#mermaid-svg-Lwf1aBTS8qpsa97g .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-Lwf1aBTS8qpsa97g .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-Lwf1aBTS8qpsa97g .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-Lwf1aBTS8qpsa97g .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-Lwf1aBTS8qpsa97g .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-Lwf1aBTS8qpsa97g .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-Lwf1aBTS8qpsa97g .marker{fill:#666;stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g .marker.cross{stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-Lwf1aBTS8qpsa97g .actor{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-Lwf1aBTS8qpsa97g text.actor > tspan{fill:#333;stroke:none;}#mermaid-svg-Lwf1aBTS8qpsa97g .actor-line{stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g #arrowhead path{fill:#333;stroke:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g .sequenceNumber{fill:white;}#mermaid-svg-Lwf1aBTS8qpsa97g #sequencenumber{fill:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g #crosshead path{fill:#333;stroke:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g .messageText{fill:#333;stroke:#333;}#mermaid-svg-Lwf1aBTS8qpsa97g .labelBox{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-Lwf1aBTS8qpsa97g .labelText,#mermaid-svg-Lwf1aBTS8qpsa97g .labelText > tspan{fill:#333;stroke:none;}#mermaid-svg-Lwf1aBTS8qpsa97g .loopText,#mermaid-svg-Lwf1aBTS8qpsa97g .loopText > tspan{fill:#333;stroke:none;}#mermaid-svg-Lwf1aBTS8qpsa97g .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:hsl(0,0%,83%);fill:hsl(0,0%,83%);}#mermaid-svg-Lwf1aBTS8qpsa97g .note{stroke:hsl(60,100%,23.3333333333%);fill:#ffa;}#mermaid-svg-Lwf1aBTS8qpsa97g .noteText,#mermaid-svg-Lwf1aBTS8qpsa97g .noteText > tspan{fill:#333;stroke:none;}#mermaid-svg-Lwf1aBTS8qpsa97g .activation0{fill:#f4f4f4;stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g .activation1{fill:#f4f4f4;stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g .activation2{fill:#f4f4f4;stroke:#666;}#mermaid-svg-Lwf1aBTS8qpsa97g:root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-svg-Lwf1aBTS8qpsa97g sequence{fill:apa;}</style><g></g><g><line id="actor6" x1="75" y1="5" x2="75" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="0" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="75" dy="0">Alice</tspan></text></g><g><line id="actor7" x1="318" y1="5" x2="318" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="243" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="318" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="318" dy="0">Bob</tspan></text></g><g><line id="actor8" x1="539" y1="5" x2="539" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="464" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="539" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="539" dy="0">John</tspan></text></g><defs><marker id="arrowhead" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z"></path></marker></defs><defs><marker id="crosshead" markerWidth="15" markerHeight="8" orient="auto" refX="16" refY="4"><path fill="black" stroke="#000000" stroke-width="1px" d="M 9,2 V 6 L16,4 Z" style="stroke-dasharray: 0, 0;"></path><path fill="none" stroke="#000000" stroke-width="1px" d="M 0,1 L 6,7 M 6,1 L 0,7" style="stroke-dasharray: 0, 0;"></path></marker></defs><defs><marker id="filled-head" refX="18" refY="7" markerWidth="20" markerHeight="28" orient="auto"><path d="M 18,7 L9,13 L14,7 L9,1 Z"></path></marker></defs><defs><marker id="sequencenumber" refX="15" refY="15" markerWidth="60" markerHeight="40" orient="auto"><circle cx="15" cy="15" r="6"></circle></marker></defs><text x="197" y="80" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Hello Bob, how are you?</text><line x1="75" y1="113" x2="318" y2="113" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="fill: none;"></line><text x="429" y="128" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">How about you John?</text><line x1="318" y1="161" x2="539" y2="161" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="197" y="176" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">I am good thanks!</text><line x1="318" y1="209" x2="75" y2="209" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#crosshead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="429" y="224" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">I am good thanks!</text><line x1="318" y1="257" x2="539" y2="257" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#crosshead)" style="fill: none;"></line><g><rect x="564" y="267" fill="#EDF2AE" stroke="#666" width="150" height="84" rx="0" ry="0" class="note"></rect><text x="639" y="272" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">Bob thinks a long</tspan></text><text x="639" y="288" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">long time, so long</tspan></text><text x="639" y="304" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">that the text does</tspan></text><text x="639" y="320" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">not fit on a row.</tspan></text></g><text x="197" y="366" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Checking with John...</text><line x1="318" y1="399" x2="75" y2="399" class="messageLine1" stroke-width="2" stroke="none" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="307" y="414" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Yes... John, how are you?</text><line x1="75" y1="447" x2="539" y2="447" class="messageLine0" stroke-width="2" stroke="none" style="fill: none;"></line><g><rect x="0" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="75" dy="0">Alice</tspan></text></g><g><rect x="243" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="318" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="318" dy="0">Bob</tspan></text></g><g><rect x="464" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="539" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, sans-serif;"><tspan x="539" dy="0">John</tspan></text></g></svg></pre>
<p>And this will produce a flow chart:</p>
<pre class=" language-mermaid"><svg id="mermaid-svg-9Xotda3w5k5JTlNL" width="100%" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="173.71875" style="max-width: 502.1031188964844px;" viewBox="0 0 502.1031188964844 173.71875"><style>#mermaid-svg-9Xotda3w5k5JTlNL{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#000000;}#mermaid-svg-9Xotda3w5k5JTlNL .error-icon{fill:#552222;}#mermaid-svg-9Xotda3w5k5JTlNL .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-9Xotda3w5k5JTlNL .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-9Xotda3w5k5JTlNL .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-9Xotda3w5k5JTlNL .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-9Xotda3w5k5JTlNL .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-9Xotda3w5k5JTlNL .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-9Xotda3w5k5JTlNL .marker{fill:#666;stroke:#666;}#mermaid-svg-9Xotda3w5k5JTlNL .marker.cross{stroke:#666;}#mermaid-svg-9Xotda3w5k5JTlNL svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-9Xotda3w5k5JTlNL .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#000000;}#mermaid-svg-9Xotda3w5k5JTlNL .cluster-label text{fill:#333;}#mermaid-svg-9Xotda3w5k5JTlNL .cluster-label span{color:#333;}#mermaid-svg-9Xotda3w5k5JTlNL .label text,#mermaid-svg-9Xotda3w5k5JTlNL span{fill:#000000;color:#000000;}#mermaid-svg-9Xotda3w5k5JTlNL .node rect,#mermaid-svg-9Xotda3w5k5JTlNL .node circle,#mermaid-svg-9Xotda3w5k5JTlNL .node ellipse,#mermaid-svg-9Xotda3w5k5JTlNL .node polygon,#mermaid-svg-9Xotda3w5k5JTlNL .node path{fill:#eee;stroke:#999;stroke-width:1px;}#mermaid-svg-9Xotda3w5k5JTlNL .node .label{text-align:center;}#mermaid-svg-9Xotda3w5k5JTlNL .node.clickable{cursor:pointer;}#mermaid-svg-9Xotda3w5k5JTlNL .arrowheadPath{fill:#333333;}#mermaid-svg-9Xotda3w5k5JTlNL .edgePath .path{stroke:#666;stroke-width:1.5px;}#mermaid-svg-9Xotda3w5k5JTlNL .flowchart-link{stroke:#666;fill:none;}#mermaid-svg-9Xotda3w5k5JTlNL .edgeLabel{background-color:white;text-align:center;}#mermaid-svg-9Xotda3w5k5JTlNL .edgeLabel rect{opacity:0.5;background-color:white;fill:white;}#mermaid-svg-9Xotda3w5k5JTlNL .cluster rect{fill:hsl(210,66.6666666667%,95%);stroke:#26a;stroke-width:1px;}#mermaid-svg-9Xotda3w5k5JTlNL .cluster text{fill:#333;}#mermaid-svg-9Xotda3w5k5JTlNL .cluster span{color:#333;}#mermaid-svg-9Xotda3w5k5JTlNL div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(-160,0%,93.3333333333%);border:1px solid #26a;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-svg-9Xotda3w5k5JTlNL:root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-svg-9Xotda3w5k5JTlNL flowchart{fill:apa;}</style><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath LS-A LE-B" id="L-A-B" style="opacity: 1;"><path class="path" d="M109.07471885813149,67.7890625L170.0546875,38.859375L246.125,38.859375" marker-end="url(https://stackedit.io/app#arrowhead9)" style="fill:none"></path><defs><marker id="arrowhead9" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-A LE-C" id="L-A-C" style="opacity: 1;"><path class="path" d="M109.07471885813149,113.7890625L170.0546875,142.71875L226.921875,142.71875" marker-end="url(https://stackedit.io/app#arrowhead10)" style="fill:none"></path><defs><marker id="arrowhead10" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-B LE-D" id="L-B-D" style="opacity: 1;"><path class="path" d="M307.84375,38.859375L352.046875,38.859375L399.9844675160073,68.85146922105326" marker-end="url(https://stackedit.io/app#arrowhead11)" style="fill:none"></path><defs><marker id="arrowhead11" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-C LE-D" id="L-C-D" style="opacity: 1;"><path class="path" d="M327.046875,142.71875L352.046875,142.71875L399.98446845691865,113.72665519397928" marker-end="url(https://stackedit.io/app#arrowhead12)" style="fill:none"></path><defs><marker id="arrowhead12" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(170.0546875,38.859375)" style="opacity: 1;"><g transform="translate(-31.8671875,-13)" class="label"><rect rx="0" ry="0" width="63.734375" height="26"></rect><foreignObject width="63.734375" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-A-B" class="edgeLabel L-LS-A' L-LE-B">Link text</span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-A-C" class="edgeLabel L-LS-A' L-LE-C"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-B-D" class="edgeLabel L-LS-B' L-LE-D"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-C-D" class="edgeLabel L-LS-C' L-LE-D"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node default" id="flowchart-A-40" transform="translate(60.59375,90.7890625)" style="opacity: 1;"><rect rx="0" ry="0" x="-52.59375" y="-23" width="105.1875" height="46" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-42.59375,-13)"><foreignObject width="85.1875" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Square Rect</div></foreignObject></g></g></g><g class="node default" id="flowchart-B-41" transform="translate(276.984375,38.859375)" style="opacity: 1;"><circle x="-30.859375" y="-23" r="30.859375" class="label-container"></circle><g class="label" transform="translate(0,0)"><g transform="translate(-20.859375,-13)"><foreignObject width="41.71875" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Circle</div></foreignObject></g></g></g><g class="node default" id="flowchart-C-43" transform="translate(276.984375,142.71875)" style="opacity: 1;"><rect rx="5" ry="5" x="-50.0625" y="-23" width="100.125" height="46" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-40.0625,-13)"><foreignObject width="80.125" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Round Rect</div></foreignObject></g></g></g><g class="node default" id="flowchart-D-45" transform="translate(435.57500076293945,90.7890625)" style="opacity: 1;"><polygon points="58.528125,0 117.05625,-58.528125 58.528125,-117.05625 0,-58.528125" transform="translate(-58.528125,58.528125)" class="label-container"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-32.03125,-13)"><foreignObject width="64.0625" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Rhombus</div></foreignObject></g></g></g></g></g></g></svg></pre>

