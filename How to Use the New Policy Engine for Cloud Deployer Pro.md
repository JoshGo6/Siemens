# How to Use the New Policy Engine for Cloud Deployer Pro

 The purpose of policies is support the ideal of *immutability*, a state where changes to an application outside of a deployment pipeline are impossible. To adhere as close as possible to this ideal, a policy has the following life cycle: definition → integration → evaluation → enforcement → drift detection.

The new policy engine for Cloud Deployer Pro is now available to support that ideal better than ever. You can now enforce all of your security and compliance rules directly in your deployment pipelines, across all your cloud and on-prem infrastructure. This new approach to policy is not just a series of "if this, then that" checks. Now you use declarative states and benefit from continuous enforcement that actively implements drift detection. 

## Define constraints and violations

The main document to define your policy is *policy-manifest.yaml*, which is referenced by *pipeline.yml*. The *policy-manifest.yaml* file defines the desired state for resources and configurations. Write your policies in a simplified YAML DSL format that compiles down to Rego. The policies define constraints and violations. A constraint might be "all S3 buckets must be encrypted with KMS keys from this specific ARN," or "no public ingress to databases." A *violation* is what happens if a constraint is not met—for example, denying the deployment, warning the administrator and logging the action, or auto-remediating the action.

> [!caution]
> Remediation is still experimental for some resource types, so use this functionality with caution.

# Implement top-level policies

Use the top-level `policies` block in *pipeline.yml* to specify which *policy-manifest.yaml* files to apply, and at which stages. You have the flexibility to specify different policies for dev, staging, and prod. For example your dev policies might only warn on some violations, while your prod policies prevent the violations from taking place.

You can override specific policy parameters per stage, which is important when you have environment-specific tag requirements or resource size limits. Additionally, policies can mandate specific tags (for example, `Owner`, `CostCenter`, or `Environment`) on all deployed resources. If a deployment tries to deploy something without the required tags, the policy blocks it. 

> [!tip]
> The tagging feature is extremely useful for cost management and accountability.

The fastest way to work with policies is through the Cloud Deployer Pro CLI. To validate a policy definition, use the following command:

```cdp
cdp policy validate
```

To run a pipeline with policy checking enabled, use the following command:

```cdp
cdp pipeline run --policy-check
```

To enforce a policy, use the following command:

```cdp
cdp policy enforce
```

## Configure read-only access to resources

 For policy enforcement to work across cloud environments, the policy engine needs read-only access to all resources that the pipeline might touch—even before deployment—to evaluate the desired state against the existing state. This requires that more granular IAM roles/service accounts be configured for the policy engine itself, separate from the roles/service accounts for deployment. In other words, cross-account/cross-cloud role assumption is key here. To configure this access, navigate to **Settings** > **Cloud Integrations**, and then use the **Policy Engine IAM Role** section. 
 
## Policy sets

Instead of one, large *policy-manifest.yaml* file, you can now define *policy sets*—modular policy files (for example, *security-policies.yaml*, *compliance-policies.yaml*, *cost-policies.yaml*) and compose them into a *policy-set.yaml* file that is referenced by *pipeline.yml*. This approach promotes reusability and increases maintainability.

> [!tip]
> Use the CLI for this feature. In the future, this functionality will be rolled out in the UI, as well.

## Apply policies conditionally

In addition to simple policies that are always active, you can define conditional policies. For example, a policy might only apply if the target environment is prod *and* the application type is database. Implement conditional policies using condition blocks in *policy-manifest.yaml*.

> [!note]
> This is an advanced feature for people with substantial experience with YAML files and cloud APIs.

## Configure Drift Detection

The policy engine contains a new drift detection feature. After a successful deployment, the policy engine continuously monitors the deployed resources against *policy-manifest.yaml* and the declared state of *pipeline.yml*. If something changes outside the pipeline (for example, someone manually opens an S3 bucket to the public), the policy engine flags the action as drift, and administrators get alerts  that are configurable via webhooks or SNS/SQS integrations. To implement drift detection, use the following command in the Cloud Deployer Pro CLI:

```cdp
detect-drift --pipeline <id>
```

> [!note]
> Resolve drift manually or by re-running the pipeline.
