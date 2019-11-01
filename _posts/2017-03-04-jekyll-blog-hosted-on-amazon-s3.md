---
layout: post
title:  "Jekyll blog hosted on Amazon S3"
date:   2017-03-04 07:55:09 +0530
---

I decided to use [Jekyll](http://jekyllrb.com/) static site generator for this blog and host it on the highly scalable and durable [Amazon S3](https://aws.amazon.com/s3/) storage. I have used Wordpress extensively and that have always been a default blogging platform of my choice. But I wanted to play around with Jekyll and test this use case.

There are significant advantages of using a static website - extremely fast as it would not have to go back to the database to fetch every content and has minimal maintenance. There are other static website generators but my choice of Jekyll was simply because of its reputation of powering the GitHub Pages.

While I decided to use Amazon S3 as my choice for hosting, [GitHub Pages](https://pages.github.com/) allows you to host static Jekyll generated websites for free.

I wanted a very simple interface for my writing, taking the technology out of the way and yet use that extensively in the background. The tools and workflow I used to meet these requirements are:

* [Markdown](https://daringfireball.net/projects/markdown/), the text-to-html conversion tool for writing in a simple text format and easy to remember syntaxes that come naturally. No menus to reach out to for formatting.
* [MacDown](http://macdown.uranusjr.com/), the OpenSource Markdown editor for Mac. I love its minimalistic interface. I am a fan of minimalism and minimalistic designs.
* [Sublime Text](https://www.sublimetext.com/) for all my coding needs though I default to using vi ever so often for quick needs!
* Git and [AWS CodeCommit](https://aws.amazon.com/codecommit/), to version control all code/text and then push those up to Amazon's CodeCommit, the private Git repository through AWS CLI. CodeCommit helps to have all the codes backed up in AWS Cloud.
* [Jekyll](http://jekyllrb.com/), installed locally and runs in the background. Whenever I write in MacDown a browser window shows the preview.
* [s3_website](https://github.com/laurilehmijoki/s3_website), to deploy my Jekyll generated static pages to S3 bucket. In my exploration so far, I find that s3_website is the best tool to upload the static contents to S3 webhosting, primarily benefitting from its native Jekyll support and GZip compression to upload into S3.
* [Amazon S3](https://aws.amazon.com/s3/) for hosting the static website.
* [AWS Account](https://aws.amazon.com/) for using all the latest cloud enabled technology and its powerful ecosystem. My AWS account is not any more on the AWS Free Tier, so cost control and optimisation is always at the back of my mind when I am working on any prototypes or solutions. For this test site so far, in AWS I have used IAM, S3, CodeCommit, Route 53, for my domain's hosted zone and AWS CLI.

Phew! So much for hosting a few static pages of a blog, you might argue. But if you abstract the underlying technology all you have is - Write your blog in an editor of your choice on your laptop, while being online or offline. When done, run two scripts when connected to internet and you are done. Your blog is published to the world. The complex technology should simplify our life at the end of the day.

Here is the architecture diagram:

![Website Architecture]({{ "/assets/website_architecture_v1-1.png" }})


If you are looking to go down this route here is the how-to on the configurations I did.

## How to host a Jekyll blog on Amazon S3 ##

### Configurations on AWS: ###
Setup an AWS account and below are the key configuration items.

### 1. Hosting the website on S3 ###
The first step is to setup [Amazon S3](https://aws.amazon.com/s3/) buckets and enable static website hosting.

Created a bucket named `blog.shouvikbasak.com`. This will be the primary bucket that will hold the website data. It is important to note that the bucket name must be the same as the domain name. So if you are configuring a blog named `example.com`, the S3 bucket name must be `example.com`.

We would also need the website traffic directed to ​`www.example.com` to be resolved to the s3 bucket. Way to do this is to create another S3 bucket with name `www.example.com`. Configure the `www.example.com` bucket to redirect to `example.com`.

Configure policy for the `blog.shouvikbasak.com` bucket to allow public view access:

```
{
"Version":"2012-10-17",
"Statement":[{
"Sid":"AddPerm",
"Effect":"Allow",
"Principal": "*",
"Action":["s3:GetObject"],
"Resource":["arn:aws:s3:::blog.shouvikbasak.com/*"
            ]}]
}
```

Here is a detailed instruction from AWS on [Setting up a static website using S3 and custom domain](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html).

### 2. Configuring custom domain in Route 53 ###
AWS Console => Route53 => DNS Management => Hosted Zones => Create Hosted Zone

```
Enter Domain Name: blog.shouvikbasak.com
Type: Public Hosted Zone
```

This would create 4 NS records:

```
ns-1849.awsdns-39.co.uk.
ns-1341.awsdns-39.org.
ns-710.awsdns-24.net.
ns-383.awsdns-47.com.
```

My domain registrar is GoDaddy. So the GoDaddy Name Servers need to be replaced by the AWS Route 53 NS records to allow delegated authority.

Logged in to Godaddy and go to the Domain Management section and choose blog.shouvikbasak.com.

Go to settings => Name Server => Manage => Choose Custom and enter the detail of the custom name servers. These would be the detail of the 4 NS records above. Ensure to remove the trailing dots (.).

```
ns-1849.awsdns-39.co.uk
ns-1341.awsdns-39.org
ns-710.awsdns-24.net
ns-383.awsdns-47.com
```

Now in Route 53 create an Alias record for the apex domain and `www` pointing to the S3 bucket endpoints hosting the site. Note that Route 53 does not charge for queries to Alias records pointing to S3 buckets that host a website.

### 3. Congiuring AWS CodeCommit ###
If you are already using Git and GitHub, using AWS CodeCommit is no different. AWS CodeCommit is a fully managed _private_ Git repository which is highly scalable and can hold codes, documents, images and other required assets. CodeCommit is also free for the first 5 active users. Check out the latest pricing [here](https://aws.amazon.com/codecommit/pricing/).

The reason I use CodeCommit is because it is a private repository. Whereas I can always share the completed codes in GitHub, my in-work projects can always stay on CodeCommit and allow me to collaborate securely. Also it comes with the benefit of easily being integrated with other AWS tools like IAM, CodeDeploy, Lambda and effectively helping with a continuous deployment approach and automation.

Setup a repository in CodeCommit for this website code and contents. Note that the S3 bucket only holds the generated pages from Jekyll. So this repository holds the actual code, enables access from multiple devices and act as a backup in the Cloud.

Check out the step by step [documentation](http://docs.aws.amazon.com/codecommit/latest/userguide/setting-up.html) on how to setup CodeCommit.

With the AWS configurations completed now is the time to setup the local environment on the laptop.

## Configurations on local device ##
All my configurations are on Mac, so the notes below are aligned to Mac. For a Ubuntu/Linux or Windows it would be pretty similar and there would be equivalent tools.

### 1. Configure the AWS CLI ###
Install Python and pip if already not installed on the laptop. AWS CLI needs Python as a prerequisite.

Install pip:

```
curl -O https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user
```

Install the AWS CLI:

```
pip3 install --upgrade --user awscli
```

Update the PATH.

A step-by-step guide is available [here](http://docs.aws.amazon.com/cli/latest/userguide/installing.html).

Configure the AWS CLI with `aws configure` and enter the detail for the IAM user that will connect through the CLI:

```
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:
```

Check [here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for more detailed instructions.

### 2. Configure Git and clone the CodeCommit repository ###
Install [Git](https://git-scm.com/) locally.

Login to AWS Console => CodeCommit. Select the repository created for this website. From `Clone URL` select HTTPS (or SSH) and copy the URL.

Clone the repository locally:

```
$ cd
$ git clone
```
### 3. Install editors ###
Install the Markdown editor and code editor of your choice.

### 4. Install and configure Jekyll ###
Install Jekyll using command `gem install jekyll bundler`

Go to the local repo directory cloned in above step.

Create Jekyll site `jekyll new my-site-name`

To run jekyll:

```
cd my-site-name
$ bundle exec jekyll serve
```

This would run the jekyll site generator locally and the site is visible at `http://localhost:4000`

Detailed instructions are available at [Jekyllrb.com](http://jekyllrb.com/docs/installation/).

Once Jekyll is installed, any blog written can be previewed locally before pushing it to the S3 bucket.

The Jekyll site has very crisp instruction set on how to [write a post](http://jekyllrb.com/docs/posts/) and geting started with Jekyll.

For the site template I used `minima`, which is Jekyll's default gem based theme. At this point my focus is not on the site design. I just need a template that is minimalist and responsive and for now `minima` fits the need perfectly. Used `bundle show minima` to check where minima is installed in the Library. This information would be required for any customisation.

### 5. Install and configure s3_website ###
Once the first post is created, previewed locally at http://localhost:4000, there are two steps from here:
* Push this to the CodeCommit repo with git add, git commit and git push. Note that to be able to access the CodeCommit repo from the command line, authenticaltion keys are required, which needs to be generated for the user in the IAM Console => select user => genearte user name and password in the section: HTTPS Git credentials for AWS CodeCommit.
* Use s3_website to push the updates to the S3 bucket from where the site will be live and accessible publicly.

Install and configure s3_website:

s3_website detail are available in this [GitHub repository](https://github.com/laurilehmijoki/s3_website)

s3_website needs Ruby and Java as a prerequisite.

Install Ruby `sudo apt-get install ruby-full`

If Java SDK is not installed download and install the Java SDK from the web.

Now install s3_website `sudo gem install s3_website`

In the web site directory configure s3_website by updating the configurations in the file `_config.yml`:

```
s3_id: ****
s3_secret: ****
s3_bucket: my-website-bucket
max_age: 86400
gzip: true
s3_endpoint:
```

The static pages to be uploaded are generated using `JEKYLL_ENV=production bundle exec jekyll serve`

From the web site directory push the site to the S3 bucket using `s3_website push`.

The site is live now!