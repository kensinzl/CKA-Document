---


---

<h1 id="solution-architect-associate">Solution Architect Associate</h1>
<ul>
<li><a href="https://www.aws.training/">AWS Cloud Practitioner Essentials</a></li>
<li><a href="https://aws.amazon.com/certification/certification-prep/?cta=saa_examprep">https://aws.amazon.com/certification/certification-prep/?cta=saa_examprep</a></li>
<li><a href="https://instant.1point3acres.com/thread/690305">https://instant.1point3acres.com/thread/690305</a></li>
<li><a href="https://medium.com/cloud-guru-%E7%9A%84%E5%BE%81%E9%80%94/%E8%80%83%E8%A9%A6%E7%AF%87-%E4%B8%89%E5%80%8B%E6%9C%88%E8%80%83%E9%81%8E-aws-solution-architect-associate-saa-c02-%E8%80%83%E8%A9%A6%E9%87%8D%E9%BB%9E%E8%88%87%E5%BF%83%E5%BE%97-%E7%B7%9A%E4%B8%8A%E8%80%83%E8%A9%A6%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A0%85-f01a9bc29c4a">https://medium.com/cloud-guru-的征途/考試篇-三個月考過-aws-solution-architect-associate-saa-c02-考試重點與心得-線上考試注意事項-f01a9bc29c4a</a></li>
<li><a href="https://blog.just4test.net/aws-certified-solutions-architect-professional-learning-note">https://blog.just4test.net/aws-certified-solutions-architect-professional-learning-note</a></li>
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
<li>you can only add 1 SQS or SNS or Lamda at a time for Amazon S3 events notification. If you need to send the events to multiple subscribers, you should implement a message fanout pattern with Amazon SNS and Amazon SQS.</li>
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
<li>Buckets even is a global service(aws-console), but is a region level</li>
<li>Objects(file) have a Key which the FULL path</li>
<li>Objects has the version ID to track, and best with version to protect your unintended delete. Restore or rollback</li>
</ul>
<h3 id="s3-bucket-version">S3-Bucket Version</h3>
<ul>
<li>delete the particular version, it is permanent delete</li>
<li>delete the file without toggle the version, then it just adds the delete marker, so it has a new version id and it is not gone</li>
<li>if you want to restore the delete marker object file, just delete the delete-marker file with particular version, then the second latest file will back.</li>
</ul>
<h3 id="cors">CORS</h3>
<ul>
<li>An origin is a schema(protocol), host(domain) and port</li>
<li>CORS means Cross-Origin Resource Sharing, means get resource from different origin</li>
<li>Same origin: <a href="http://emaple.com/app">http://emaple.com/app</a> &amp; <a href="http://exaple.com/app2">http://exaple.com/app2</a></li>
<li>Differnent origin: <a href="http://www.example.com">http://www.example.com</a> &amp; <a href="http://other.example.com">http://other.example.com</a>, it may block unless you have the correct CORS header</li>
<li>The request won’t be fulfilled unless the other origin allows for the request, using CORS Header(ex: Access-Control-Allow-Origin)</li>
<li>The CORS header have to be set on the Corss origin. Eg: Client A hitting Server A and then from Server A to Server B, the Server B need to set the CORS header for the Server A URL.</li>
</ul>
<h3 id="s3-lifecycle-rules">S3 lifecycle rules</h3>
<ul>
<li>It defines when objects are transitioned to another storage class.<br>
eg: move objects to Standard IA class 60 days after creation<br>
move to Glacier for arcgiving after 6 momths</li>
<li>Configure objects to expire(delete) after some time<br>
eg: access log files can be set to delete after a 365 days</li>
</ul>
<h3 id="s3-event">S3 event</h3>
<ul>
<li>SQS</li>
<li>SNS</li>
<li>Lamda</li>
</ul>
<h3 id="s3-sqs">S3 SQS</h3>
<ul>
<li>Visibility time: the consumer should finish the message process and delete the message among this time, otherwise, the message will be visiable agaion for other consumers.</li>
<li>The visibility timeout is a period of time during which Amazon SQS prevents other consuming components from receiving and processing a message. When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn’t automatically delete the message. Because Amazon SQS is a distributed system, there’s no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application). Thus, the consumer must delete the message from the queue after receiving and processing it. Immediately after the message is received, it remains in the queue. To prevent other consumers from processing the message again, Amazon SQS sets a <strong><em>visibility timeout</em></strong>, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message. The default visibility timeout for a message is 30 seconds. The maximum is 12 hours.<br>
<a href="https://medium.com/event-driven-utopia/aws-sqs-visibility-timeout-explained-c13d8a728ab5">https://medium.com/event-driven-utopia/aws-sqs-visibility-timeout-explained-c13d8a728ab5</a></li>
</ul>
<h3 id="serverless">Serverless</h3>
<ul>
<li>serverless us a bew paradigm in which the developers do not have to manage server anymore</li>
<li>you just deploy the funtions, serverless = function as a service</li>
<li>serverless does not mean no server - just mean you do not need or manage them</li>
</ul>
<h4 id="iam-identity-access-management">IAM Identity Access Management</h4>
<ul>
<li>Users &amp; Group, group contains users but can not contain group</li>
<li>User does not need to belong to group, and also can across multiple groups</li>
<li>IAM is a global service not for the region</li>
<li>User &amp; Group &amp; Role</li>
</ul>
<h3 id="iam-role">IAM Role</h3>
<ul>
<li><a href="https://explore.skillbuilder.aws/learn/course/134/play/484/aws-cloud-practitioner-essentials">https://explore.skillbuilder.aws/learn/course/134/play/484/aws-cloud-practitioner-essentials</a></li>
</ul>
<blockquote>
<p>In the coffee shop, an employee rotates to different workstations throughout the day. Depending on the staffing of the coffee shop, this employee might perform several duties: work at the cash register, update the inventory system, process online orders, and so on.</p>
</blockquote>
<blockquote>
<p>When the employee needs to switch to a different task, they give up their access to one workstation and gain access to the next workstation. The employee can easily switch between workstations, but at any given point in time, they can have access to only a single workstation. This same concept exists in AWS with IAM roles.</p>
</blockquote>
<blockquote>
<p>An IAM role is an identity that you can assume to gain temporary access to permissions.</p>
</blockquote>
<blockquote>
<p>Before an IAM user, application, or service can assume an IAM role, they must be granted permissions to switch to the role. When someone assumes an IAM role, they abandon all previous permissions that they had under a previous role and assume the permissions of the new role.</p>
</blockquote>
<ul>
<li>
<p>User - End user think about people</p>
</li>
<li>
<p>Groups- a set of users under one set of permission(policies)</p>
</li>
<li>
<p>Roles - are used to grant specific permission to specific actors for a set of duration of time. These actors can be  <strong>authenticated by AWS or some trusted external system.</strong></p>
</li>
</ul>
<p>User and roles use policies for authorization. Keep in mind that user and role can’t do anything until you allow certain actions with a policy.</p>
<p>Answer the following questions and you will differentiate between a user and a role:</p>
<ul>
<li>Can have a password? Yes-&gt; user, No-&gt; role</li>
<li>Can have an access key? Yes-&gt; user, No-&gt; role</li>
<li>Can belong to a group? Yes-&gt; user, No -&gt; role</li>
</ul>
<h3 id="aurora">Aurora</h3>
<ul>
<li>global distributed application to span multiple AWS regions</li>
<li>recovery point objective and recovery time objective</li>
</ul>
<h3 id="dynamodb">DynamoDB</h3>
<ul>
<li>NoSQL</li>
</ul>
<blockquote>
<p>Create an API using Amazon API Gateway and use AWS Lambda to handle the bursts of traffic in seconds<br>
Create an API using Amazon API Gateway and use an Auto Scaling group of Amazon EC2 instance to handle the bursts of traffic in senconds<br>
-&gt; both all make sense, but Lambda is faster than EC2</p>
</blockquote>
<p>AWS X-Ray<br>
-&gt; debug and analyze micro-service root cause issue and performance</p>
<p>Amazon DynamoDB Accelerator and Amazon ElastiCache can boost the DynamoDB performance<br>
DAX has more performance</p>
<p>DynamoDB Stream which you have to manually enable, and wheneven a change action, the DynamonDB can sent the before and change info</p>
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
<pre class=" language-mermaid"><svg id="mermaid-svg-hX9pnqqG2F3eqH22" width="100%" xmlns="http://www.w3.org/2000/svg" height="543" style="max-width: 814px;" viewBox="-50 -10 814 543"><style>#mermaid-svg-hX9pnqqG2F3eqH22{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#000000;}#mermaid-svg-hX9pnqqG2F3eqH22 .error-icon{fill:#552222;}#mermaid-svg-hX9pnqqG2F3eqH22 .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-hX9pnqqG2F3eqH22 .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-hX9pnqqG2F3eqH22 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-hX9pnqqG2F3eqH22 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-hX9pnqqG2F3eqH22 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-hX9pnqqG2F3eqH22 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-hX9pnqqG2F3eqH22 .marker{fill:#666;stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22 .marker.cross{stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-hX9pnqqG2F3eqH22 .actor{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-hX9pnqqG2F3eqH22 text.actor > tspan{fill:#333;stroke:none;}#mermaid-svg-hX9pnqqG2F3eqH22 .actor-line{stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22 .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 #arrowhead path{fill:#333;stroke:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 .sequenceNumber{fill:white;}#mermaid-svg-hX9pnqqG2F3eqH22 #sequencenumber{fill:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 #crosshead path{fill:#333;stroke:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 .messageText{fill:#333;stroke:#333;}#mermaid-svg-hX9pnqqG2F3eqH22 .labelBox{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-hX9pnqqG2F3eqH22 .labelText,#mermaid-svg-hX9pnqqG2F3eqH22 .labelText > tspan{fill:#333;stroke:none;}#mermaid-svg-hX9pnqqG2F3eqH22 .loopText,#mermaid-svg-hX9pnqqG2F3eqH22 .loopText > tspan{fill:#333;stroke:none;}#mermaid-svg-hX9pnqqG2F3eqH22 .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:hsl(0,0%,83%);fill:hsl(0,0%,83%);}#mermaid-svg-hX9pnqqG2F3eqH22 .note{stroke:hsl(60,100%,23.3333333333%);fill:#ffa;}#mermaid-svg-hX9pnqqG2F3eqH22 .noteText,#mermaid-svg-hX9pnqqG2F3eqH22 .noteText > tspan{fill:#333;stroke:none;}#mermaid-svg-hX9pnqqG2F3eqH22 .activation0{fill:#f4f4f4;stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22 .activation1{fill:#f4f4f4;stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22 .activation2{fill:#f4f4f4;stroke:#666;}#mermaid-svg-hX9pnqqG2F3eqH22:root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-svg-hX9pnqqG2F3eqH22 sequence{fill:apa;}</style><g></g><g><line id="actor3" x1="75" y1="5" x2="75" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="0" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="75" dy="0">Alice</tspan></text></g><g><line id="actor4" x1="318" y1="5" x2="318" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="243" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="318" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="318" dy="0">Bob</tspan></text></g><g><line id="actor5" x1="539" y1="5" x2="539" y2="532" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="464" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="539" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="539" dy="0">John</tspan></text></g><defs><marker id="arrowhead" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z"></path></marker></defs><defs><marker id="crosshead" markerWidth="15" markerHeight="8" orient="auto" refX="16" refY="4"><path fill="black" stroke="#000000" stroke-width="1px" d="M 9,2 V 6 L16,4 Z" style="stroke-dasharray: 0, 0;"></path><path fill="none" stroke="#000000" stroke-width="1px" d="M 0,1 L 6,7 M 6,1 L 0,7" style="stroke-dasharray: 0, 0;"></path></marker></defs><defs><marker id="filled-head" refX="18" refY="7" markerWidth="20" markerHeight="28" orient="auto"><path d="M 18,7 L9,13 L14,7 L9,1 Z"></path></marker></defs><defs><marker id="sequencenumber" refX="15" refY="15" markerWidth="60" markerHeight="40" orient="auto"><circle cx="15" cy="15" r="6"></circle></marker></defs><text x="197" y="80" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Hello Bob, how are you?</text><line x1="75" y1="113" x2="318" y2="113" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="fill: none;"></line><text x="429" y="128" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">How about you John?</text><line x1="318" y1="161" x2="539" y2="161" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="197" y="176" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">I am good thanks!</text><line x1="318" y1="209" x2="75" y2="209" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#crosshead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="429" y="224" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">I am good thanks!</text><line x1="318" y1="257" x2="539" y2="257" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#crosshead)" style="fill: none;"></line><g><rect x="564" y="267" fill="#EDF2AE" stroke="#666" width="150" height="84" rx="0" ry="0" class="note"></rect><text x="639" y="272" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">Bob thinks a long</tspan></text><text x="639" y="288" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">long time, so long</tspan></text><text x="639" y="304" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">that the text does</tspan></text><text x="639" y="320" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="noteText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 14px; font-weight: 400;"><tspan x="639">not fit on a row.</tspan></text></g><text x="197" y="366" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Checking with John...</text><line x1="318" y1="399" x2="75" y2="399" class="messageLine1" stroke-width="2" stroke="none" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="307" y="414" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Yes... John, how are you?</text><line x1="75" y1="447" x2="539" y2="447" class="messageLine0" stroke-width="2" stroke="none" style="fill: none;"></line><g><rect x="0" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="75" dy="0">Alice</tspan></text></g><g><rect x="243" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="318" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="318" dy="0">Bob</tspan></text></g><g><rect x="464" y="467" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="539" y="499.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="539" dy="0">John</tspan></text></g></svg></pre>
<p>And this will produce a flow chart:</p>
<pre class=" language-mermaid"><svg id="mermaid-svg-c3HWKPVFJy4WduCM" width="100%" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="174.4375" style="max-width: 502.72735595703125px;" viewBox="0 0 502.72735595703125 174.4375"><style>#mermaid-svg-c3HWKPVFJy4WduCM{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#000000;}#mermaid-svg-c3HWKPVFJy4WduCM .error-icon{fill:#552222;}#mermaid-svg-c3HWKPVFJy4WduCM .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-c3HWKPVFJy4WduCM .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-c3HWKPVFJy4WduCM .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-c3HWKPVFJy4WduCM .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-c3HWKPVFJy4WduCM .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-c3HWKPVFJy4WduCM .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-c3HWKPVFJy4WduCM .marker{fill:#666;stroke:#666;}#mermaid-svg-c3HWKPVFJy4WduCM .marker.cross{stroke:#666;}#mermaid-svg-c3HWKPVFJy4WduCM svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-c3HWKPVFJy4WduCM .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#000000;}#mermaid-svg-c3HWKPVFJy4WduCM .cluster-label text{fill:#333;}#mermaid-svg-c3HWKPVFJy4WduCM .cluster-label span{color:#333;}#mermaid-svg-c3HWKPVFJy4WduCM .label text,#mermaid-svg-c3HWKPVFJy4WduCM span{fill:#000000;color:#000000;}#mermaid-svg-c3HWKPVFJy4WduCM .node rect,#mermaid-svg-c3HWKPVFJy4WduCM .node circle,#mermaid-svg-c3HWKPVFJy4WduCM .node ellipse,#mermaid-svg-c3HWKPVFJy4WduCM .node polygon,#mermaid-svg-c3HWKPVFJy4WduCM .node path{fill:#eee;stroke:#999;stroke-width:1px;}#mermaid-svg-c3HWKPVFJy4WduCM .node .label{text-align:center;}#mermaid-svg-c3HWKPVFJy4WduCM .node.clickable{cursor:pointer;}#mermaid-svg-c3HWKPVFJy4WduCM .arrowheadPath{fill:#333333;}#mermaid-svg-c3HWKPVFJy4WduCM .edgePath .path{stroke:#666;stroke-width:1.5px;}#mermaid-svg-c3HWKPVFJy4WduCM .flowchart-link{stroke:#666;fill:none;}#mermaid-svg-c3HWKPVFJy4WduCM .edgeLabel{background-color:white;text-align:center;}#mermaid-svg-c3HWKPVFJy4WduCM .edgeLabel rect{opacity:0.5;background-color:white;fill:white;}#mermaid-svg-c3HWKPVFJy4WduCM .cluster rect{fill:hsl(210,66.6666666667%,95%);stroke:#26a;stroke-width:1px;}#mermaid-svg-c3HWKPVFJy4WduCM .cluster text{fill:#333;}#mermaid-svg-c3HWKPVFJy4WduCM .cluster span{color:#333;}#mermaid-svg-c3HWKPVFJy4WduCM div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(-160,0%,93.3333333333%);border:1px solid #26a;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-svg-c3HWKPVFJy4WduCM:root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-svg-c3HWKPVFJy4WduCM flowchart{fill:apa;}</style><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath LS-A LE-B" id="L-A-B" style="opacity: 1;"><path class="path" d="M109.65503771551724,67.609375L170.04296875,38.859375L246.109375,38.859375" marker-end="url(https://stackedit.io/app#arrowhead5)" style="fill:none"></path><defs><marker id="arrowhead5" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-A LE-C" id="L-A-C" style="opacity: 1;"><path class="path" d="M109.65503771551724,114.328125L170.04296875,143.078125L226.90625,143.078125" marker-end="url(https://stackedit.io/app#arrowhead6)" style="fill:none"></path><defs><marker id="arrowhead6" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-B LE-D" id="L-B-D" style="opacity: 1;"><path class="path" d="M307.828125,38.859375L352.03125,38.859375L400.08636308747873,68.91363843840007" marker-end="url(https://stackedit.io/app#arrowhead7)" style="fill:none"></path><defs><marker id="arrowhead7" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath LS-C LE-D" id="L-C-D" style="opacity: 1;"><path class="path" d="M327.03125,143.078125L352.03125,143.078125L400.0863612053901,114.023862731269" marker-end="url(https://stackedit.io/app#arrowhead8)" style="fill:none"></path><defs><marker id="arrowhead8" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(170.04296875,38.859375)" style="opacity: 1;"><g transform="translate(-31.86328125,-13.359375)" class="label"><rect rx="0" ry="0" width="63.7265625" height="26.71875"></rect><foreignObject width="63.7265625" height="26.71875"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-A-B" class="edgeLabel L-LS-A' L-LE-B">Link text</span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-A-C" class="edgeLabel L-LS-A' L-LE-C"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-B-D" class="edgeLabel L-LS-B' L-LE-D"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><rect rx="0" ry="0" width="0" height="0"></rect><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span id="L-L-C-D" class="edgeLabel L-LS-C' L-LE-D"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node default" id="flowchart-A-24" transform="translate(60.58984375,90.96875)" style="opacity: 1;"><rect rx="0" ry="0" x="-52.58984375" y="-23.359375" width="105.1796875" height="46.71875" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-42.58984375,-13.359375)"><foreignObject width="85.1796875" height="26.71875"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Square Rect</div></foreignObject></g></g></g><g class="node default" id="flowchart-B-25" transform="translate(276.96875,38.859375)" style="opacity: 1;"><circle x="-30.859375" y="-23.359375" r="30.859375" class="label-container"></circle><g class="label" transform="translate(0,0)"><g transform="translate(-20.859375,-13.359375)"><foreignObject width="41.71875" height="26.71875"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Circle</div></foreignObject></g></g></g><g class="node default" id="flowchart-C-27" transform="translate(276.96875,143.078125)" style="opacity: 1;"><rect rx="5" ry="5" x="-50.0625" y="-23.359375" width="100.125" height="46.71875" class="label-container"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-40.0625,-13.359375)"><foreignObject width="80.125" height="26.71875"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Round Rect</div></foreignObject></g></g></g><g class="node default" id="flowchart-D-29" transform="translate(435.8792953491211,90.96875)" style="opacity: 1;"><polygon points="58.848046875,0 117.69609375,-58.848046875 58.848046875,-117.69609375 0,-58.848046875" transform="translate(-58.848046875,58.848046875)" class="label-container"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-32.02734375,-13.359375)"><foreignObject width="64.0546875" height="26.71875"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Rhombus</div></foreignObject></g></g></g></g></g></g></svg></pre>

