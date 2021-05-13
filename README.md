# 2021-DevOps
Assignment for Accendero Software, Inc.

References:
1. AWS Codepipeline -  https://aws.amazon.com/codepipeline/?c=dv&sec=srv
2. AWS CloudFormation - https://aws.amazon.com/cloudformation/

Infrastructure as code


CI/CD & Automation
Assumptions -> The repository resides on GitHub.

1)	To create a CI/CD pipeline, we will be using the AWS CodePipeline service. Follow the instructions below.
    a.	Navigate to AWS CodePipeline using the AWS Console.
    b.	Select “create pipeline”.
    c.	Enter application name, select new service role, and enter role name.
    d.	 Next, select source provider (GitHub in this case). We will then be asked to link your GitHub Account. Once linked, select detection options. The selected detection option would then act as a trigger for the pipeline.
    e.	Next, proceed to AWS CodeBuild service. Select build provider (AWS CodeBuild in this case), select region, and click on create project.
    f.	A new tab should open. Enter the build project name, select image environment image, OS, create a new service role, select build spec and finally, select option for logs (recommended).
    g.	Before moving onto the next phase (deployment), ensure that you have EC2 instance(s) running. To provision an EC2 instance, follow the steps below. If you have already provisioned       instances, process to step ‘h’.
        i.	Inside AWS Console, navigate to EC2 service and click on “Launch Instance”.
        ii.	Select the Machine Image.
        iii.	Next, choose instance type.
        iv.	Next, select the number of instances you would like the application to be deployed on. You will also need to select the IAM role.
        v.	Next, add storage.
        vi.	After adding storage, we would have to assign tags to this instance. The tags saved here would help us identify the instance on which we would like to deploy the application.
        vii.	Add security group.
        viii.	Lastly, review and launch.
    h.	Open AWS Console on a new tab and navigate to AWS Code Deploy service.
    i.	Click on “create application” and select deploy provider (AWS CodeDeploy in this case) and region.
    j.	On the previous tab, proceed to the next phase, Code Deploy.
    k.	Here, we need to select application name (recently created) and the compute platform (EC2/OnPrem in this case).
    l.	Click on “create deployment group”.
    m.	Enter group name, service role, deployment type, environment config (EC2), install CodeDeploy agent, select deployment config and click on checkbox if load balancer is required.
    n.	Once the deployment group is created, create a new deployment.
    o.	Select the deployment group, source of artifacts (May need to connect to GitHub), enter repository name and commit ID for initial deployment.
    p.	Next, you may review the pipeline and click on create code pipeline.

    •	Please note that you need to have a buildspec.yml file in the root directory of your repository to allow AWS CodeBuild to build the application. Similarly, you need to have an appspec.yml file in the root directory for code deployment. (Dummy files have been added to this repository)
    •	There is a slight possibility that the deployment would fail. In this case, you may try to install the Deploy Agent manually by connecting to the EC2 instance and running the following commands.
        o	sudo yum update
        o	sudo yum install ruby
        o	sudo yum install wget
        o	cd /home/ec2-user
        o	wget https://bucket-name.s3.region-identifier.amazonaws.com/latest/install
        o	chmod +x ./install
        o	sudo ./install auto
    •	You may also refer to this document to View instance details with CodeDeploy - AWS CodeDeploy (amazon.com)

2)	This part would require knowledge from the previous part. Follow the steps ‘e’ and ‘f’ to automate the build process. Since both the branches, ‘devel’ and ‘master’, reside in the same repository, we can create a deployment application with two deployment groups, ‘Staging’ and ‘Production’. Follow the steps below.
    a)	Consider we have created two EC2 instances following the step ‘g’ above and we also have a deployment application with two deployment groups.
    b)	Create a new deployment in ‘Staging’ group.
    c)	Select revision type (GitHub in this case) and specify the path to ‘devel’ repository. You may enter the latest commit to start the deployment.
    d)	Similarly, create a new deployment in ‘Production’ group along with specifying the path to ‘master’ repository and the latest commit.
    e)	Navigate to AWS CodePipeline using the AWS Console and create a new pipeline. (steps ‘a’ to ‘e’ from the previous question)


Application Scaling


