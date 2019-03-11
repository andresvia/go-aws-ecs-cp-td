# go-awsecs

Library and tools for AWS ECS operations.

Get golang: https://golang.org/dl/

RTFM: https://github.com/golang/go/wiki/SettingGOPATH

# tools

## update-aws-ecs-service

This tool is inspired by [AWS CodePipeline image definitions file method for updating existing ECS services](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create.html#pipelines-create-image-definitions). This tool attempts to do something similar in a standalone fashion without depending on AWS CodePipeline, and more importantly without having to create individual AWS CodePipeline pipelines.

Get:

Grab binary distribution from [releases tab](https://git.autodesk.com/t-villa/go-awsecs/releases). Or.

```
go get -v -u git.autodesk.com/t-villa/go-awsecs/cmd/update-aws-ecs-service
```

Use<sup>1</sup>:

```
update-aws-ecs-service -h
Usage of update-aws-ecs-service:
  -cluster string
    	cluster name
  -container-envvar value
    	container-name=envvar-name=envvar-value
  -container-image value
    	container-name=image
  -container-secret value
    	container-name=secret-name=secret-valuefrom
  -desired-count int
    	desired-count (negative: no change) (default -1)
  -profile string
    	profile name
  -region string
    	region name
  -service string
    	service name
```

Example.

First, build and push a new Docker image for your service somewhere else.

```
docker build -t myrepo/myimg:newtag .
docker push myrepo/myimg:newtag
```

Then, alter the existing container image only, like AWS CodePipeline does.

```
update-aws-ecs-service \
  -cluster mycluster \
  -service myservice \
  -container-image mycontainer=myrepo/myimg:newtag
```

Alternatively, you can also alter environment variables and service desired count.

```
update-aws-ecs-service \
  -cluster mycluster \
  -service myservice \
  -container-image mycontainer=myrepo/myimg:newtag \
  -container-envvar mycontainer=envvarname=envvarvalue \
  -desired-count 1
```

## enforce-aws-ecs-asg-launchconfig

This tool is useful to ensure that all EC2 instances in a ECS cluster backed up by a ASG share the launch configuration defined in the ASG. This tool doesn't work with launch templates. ECS EC2 Container Instances will be drained. EC2 Instances will be terminated (after they have been drained).

Get:

Grab binary distribution from [releases tab](https://git.autodesk.com/t-villa/go-awsecs/releases). Or.

```
go get -v -u git.autodesk.com/t-villa/go-awsecs/cmd/enforce-aws-ecs-asg-launchconfig
```

Use:

```
enforce-aws-ecs-asg-launchconfig -h
Usage of enforce-aws-ecs-asg-launchconfig:
  -asg string
    	asg name
  -cluster string
    	cluster name
  -profile string
    	profile name
  -region string
    	region name
```

Example:

```
enforce-aws-ecs-asg-launchconfig \
  -asg myasgname \
  -cluster myclustername
# default timeout for the operation is 15 minutes
```

----

1. https://unix.stackexchange.com/a/111557/19393
