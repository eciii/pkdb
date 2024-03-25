AWS developer resources are scattered across several websites and domains. Here is an attempt to describe them in a sort of systematic way. I'll start with the principal domains:

- The [main AWS website](https://aws.amazon.com/ ), `MAIN = aws.amazon.com`
- The [AWS GitHub pages](https://aws.github.io/), `PAGES = aws.github.io`
- The [GitHub](https://github.com) domain, `GITHUB = github.com`

The main website has in turn several major sub-sites. The most relevant ones for me right now are:

- The [Developer Center](https://aws.amazon.com/developer/), `DEV = [MAIN]/developer/`
- The [Documentation](https://docs.aws.amazon.com/) subdomain, `DOCS = docs.[MAIN]`

Some documentation websites are of the form `https://[project].amazonaws.com/[version]/documentation/api/latest/index.html`. Such websites will be referred as `amazonaws` websites. They are particular because none of their sub-URLs are functional.

Now I'm are able to present the links related to the services and client tools I'm interested in.

### S3 (service)

- [Service landing page](https://aws.amazon.com/s3/), `[MAIN]/s3/`
- [Documentation landing page](https://docs.aws.amazon.com/s3/), `[DOCS]/s3/`
- Actual documentation pages (they are based on `ACTUAL_DOCS = [DOCS]/AmazonS3/latest/` but this URL alone is not functional):
	- [Guides](https://docs.aws.amazon.com/AmazonS3/latest/userguide/), `[ACTUAL_DOCS]/userguide/`
	- [References](https://docs.aws.amazon.com/AmazonS3/latest/API/), `[ACTUAL_DOCS]/API/`

### SDKs for Go and Python (client tools)

**Go**

Note that the SDK for Go has two versions: Version 1 and Version 2. Both versions seem to be currently supported. For differences between the two versions see the [announcement of the General Availability (GA) release of Version 2](https://aws.amazon.com/blogs/developer/aws-sdk-for-go-version-2-general-availability/).

- [Language landing page](https://aws.amazon.com/developer/language/go/), `[DEV]/language/go/`
- [SDK landing page](https://aws.amazon.com/sdk-for-go/), `[MAIN]/sdk-for-go/`
	- Version 2 has also its own [landing page](https://aws.github.io/aws-sdk-go-v2/), `GO_SDK_V2 = [PAGES]/aws-sdk-go-v2/`
- [Documentation landing page](https://docs.aws.amazon.com/sdk-for-go/), `GO_SDK_DOCS = [DOCS]/sdk-for-go/`
- Actual documentation pages:
	- For Version 2:
		- [Guides](https://aws.github.io/aws-sdk-go-v2/docs/), `[GO_SDK_V2]/docs/`
		- References are in [pkg.go.dev](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2)
	- For Version 1:
		- [Guides](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/), `[GO_SDK_DOCS]/v1/developer-guide/`
		- [References](https://docs.aws.amazon.com/sdk-for-go/api/), `[GO_SDK_DOCS]/api/`
- SDK source code:
	- Version 2 [source code](https://github.com/aws/aws-sdk-go-v2), `[GITHUB]/aws/aws-sdk-go-v2`
	- Version 1 [source code](https://github.com/aws/aws-sdk-go), `[GITHUB]/aws/aws-sdk-go`

**Python**

Note that the SDK for Python is also known as Boto3.

- [Language landing page](https://aws.amazon.com/developer/language/python/), `[DEV]/language/python/`
- [SDK landing page](https://aws.amazon.com/sdk-for-python/), `[MAIN]/sdk-for-python/`
- [Documentation landing page](https://docs.aws.amazon.com/pythonsdk/), `[DOCS]/pythonsdk/`
- Actual documentation pages (guides and references) are in the [Boto3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) website (an `amazonaws` website)
- [SDK source code](https://github.com/boto/boto3), `[GITHUB]/boto/boto3` (note that it is under the `boto` account, not the `aws` account)
	- The SDK uses the [botocore](https://github.com/boto/botocore) package at `[GITHUB]/boto/botocore` (also under the `boto` account), which is also used by the AWS CLI

### AWS CLI (client tool)

Note that the most recent version is version 2. Version 1 is not supported anymore.

- [Tool landing page](https://aws.amazon.com/cli/), `[MAIN]/cli/`
- [Documentation landing page](https://docs.aws.amazon.com/cli/), `CLI_DOCS = [DOCS]/cli/`
- Actual documentation pages:
	- [Guides](https://docs.aws.amazon.com/cli/latest/userguide/), `[CLI_DOCS]/latest/userguide/`
	- References are in the [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/) website (an `amazonaws` website)
- [Tool source code](https://github.com/aws/aws-cli), `[GITHUB]/aws/aws-cli`. The AWS CLI uses:
	- The [botocore](https://github.com/boto/botocore) package at `[GITHUB]/boto/botocore` (under the `boto` account, not the `aws` account), which is also used by the SDK for Python
	- The [s3transfer](https://github.com/boto/s3transfer) package at `[GITHUB]/boto/s3transfer` (under the `boto` account, not the `aws` account)