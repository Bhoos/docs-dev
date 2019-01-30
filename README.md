# Developer Documentation
Version: 1.0

## Deployment
1. Install `pip` (A python package manager)
   > `$ easy_install pip`

2. Install `mkdocs`
   > `$ pip install mkdocs --user`

3. Install `mkdocs-bootswatch` theme collection
   > `$ pip install mkdocs-bootswatch --user`

At this point you should be able to serve and build the content locally from your system.

4. Serve with hot changes update
   > `$ mkdocs serve`

5. Build static site for deployment
   > `$ mkdocs build`

To publish the site to AWS S3, make sure you have installed the AWS CLI. And create a credential
named `bhoos` in your aws credentials that has access to publish site to `dev-docs` bucket in
the S3.

6. Deploy the documentation
   > `$ yarn deploy`

