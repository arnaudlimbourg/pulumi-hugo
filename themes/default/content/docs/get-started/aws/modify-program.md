---
title: Modify the Program | AWS
h1: Modify the Program
linktitle: Modify the Program
meta_desc: This page provides an overview on how to update an AWS project from a Pulumi program.
weight: 6
menu:
  getstarted:
    parent: aws
    identifier: aws-modify-program

aliases: ["/docs/quickstart/aws/modify-program/"]
---

Now that your S3 bucket is provisioned, let's add a file to it. First, from within your project directory, create a new file called `hello.txt` file with some content in it:

{{< chooser os "macos,linux,windows" / >}}

{{% choosable os macos %}}

```bash
echo 'Hello, Pulumi!' > hello.txt
```

{{% /choosable %}}

{{% choosable os linux %}}

```bash
echo 'Hello, Pulumi!' > hello.txt
```

{{% /choosable %}}

{{% choosable os windows %}}

```powershell
$hello = "Hello, Pulumi!"
$hello | Out-File -FilePath hello.txt
```

{{% /choosable %}}

Now that you have your new `hello.txt` with some content, open your program file and modify it to add the file to your S3 bucket.

To accomplish this, you will use Pulumi's `FileAsset` class to assign the content of the file to a new  `BucketObject`:

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" / >}}

{{% choosable language javascript %}}

In `index.js`, create the `BucketObject` right after creating the bucket itself:

```javascript
const bucketObject = new aws.s3.BucketObject("hello.txt", {
    bucket: bucket.id,
    source: new pulumi.asset.FileAsset("hello.txt")
});
```

{{% /choosable %}}

{{% choosable language typescript %}}

In `index.ts`, create the `BucketObject` right after creating the bucket itself:

```typescript
const bucketObject = new aws.s3.BucketObject("hello.txt", {
    bucket: bucket.id,
    source: new pulumi.asset.FileAsset("hello.txt")
});
```

{{% /choosable %}}

{{% choosable language python %}}

In `__main__.py`, create a new bucket object by adding the following right after creating the bucket itself:

```python
bucketObject = s3.BucketObject(
    'hello.txt',
    bucket=bucket.id,
    source=pulumi.FileAsset('hello.txt')
)
```

{{% /choosable %}}

{{% choosable language go %}}

In `main.go`, create the `BucketObject` right after creating the bucket itself:

```go
_, err = s3.NewBucketObject(ctx, "hello.txt", &s3.BucketObjectArgs{
    Bucket:  bucket.ID(),
    Source: pulumi.NewFileAsset("hello.txt"),
})
if err != nil {
    return err
}
```

{{% /choosable %}}

{{% choosable language csharp %}}

In `Program.cs`, create a new `BucketObject` right after creating the bucket itself.

```csharp
var bucketObject = new BucketObject("hello.txt", new BucketObjectArgs
{
    Bucket = bucket.BucketName,
    Source = new FileAsset("./hello.txt")
});
```

{{% /choosable %}}

{{% choosable language java %}}

In {{< langfile >}}, import the `FileAsset`, `BucketObject`, and `BucketObjectArgs` classes, then create the `BucketObject` right after creating the bucket itself.

```java
// ...
import com.pulumi.asset.FileAsset;
import com.pulumi.aws.s3.BucketObject;
import com.pulumi.aws.s3.BucketObjectArgs;

public class App {
    public static void main(String[] args) {
        Pulumi.run(ctx -> {
            // var bucket = ...

            // Create an S3 Bucket object
            new BucketObject("hello.txt", BucketObjectArgs.builder()
                .bucket(bucket.id())
                .source(new FileAsset("hello.txt"))
                .build()
            );
```

{{% /choosable %}}

{{% choosable language "yaml" %}}

In {{< langfile >}}, create the `BucketObject` right below the bucket itself.

```yaml
resources:
  # ...
  hello.txt:
    type: aws:s3:BucketObject
    properties:
      bucket: ${my-bucket}
      source:
        Fn::FileAsset: ./hello.txt
```

{{% /choosable %}}

This bucket object is part of the `Bucket` that we deployed earlier because we _reference_ the bucket name in the properties of the bucket object.

We refer to this relationship as the `BucketObject` being a _child_ resource of the S3 `Bucket` that is the _parent_ resource. This is how Pulumi knows what S3 bucket the object should live in.

Next, you'll deploy your changes.

{{< get-started-stepper >}}
