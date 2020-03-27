---
description: >-
  Kubernetes Operator written in Go to reconcile Segment Job custom resources
  definition (CRD).
---

# Segment Manager

The Segment Manager is a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) which reconciles on a custom resource definition for segment jobs. Its main purpose is to ensure that the segment cronjob is up-to-date after any `create`,  `update` or `delete` request against the Kubernetes API.

It uses the [Segment Table Creation](segment-table-creation.md) script to start jobs which create [Segments](../../../learning-grnry-1/granary-glossary.md).

A CRD model is provided to improve creation of Segments called `SegmentSpec`. This model holds all the configuration attributes from the Segment Table Creation script.  

This model allows to create Segment Custom Resources via the K8s API or use the [Segment Management API](../../api-reference/segment-management-api.md) . 

The Segment Manager uses these [custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) to create a Segment CronJob equal to using the Segment Table Creation script directly via helm.



## Model SegmentSpec

The `SegmentSpec` is an object with following attributes `cronjob`, `env`, `envFromSecrets` and `image` . Each attribute is an object itself described below. All optional attributes can be set via [env variables ](https://gitlab.alvary.io/grnry/segment-manager/-/blob/master/pkg/apis/segment/v1alpha1/envs.go)in the Segment Manager to overwrite the defaults.

{% hint style="info" %}
An example SegmentSpec can be found [here](https://gitlab.alvary.io/grnry/segment-manager/-/blob/master/deploy/crds/segment.grnry.io_v1alpha1_segment_cr.yaml) .
{% endhint %}

### Cronjob

| Attribute | Description | Default | Required/Optional |
| :--- | :--- | :--- | :--- |
| schedule | Crontab defining the interval of the resulting job. | - | Required |
| concurrencyPolicy | [Kubernetes Docs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) | Always | Optional |
| failedJobsHistoryLimit | [Kubernetes Docs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) | 1 | Optional |
| successfulJobsHistoryLimit | [Kubernetes Docs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) | 1 | Optional |

### Env

The Env variables allow to configure what the Segment Table Creation script is doing. 

{% hint style="info" %}
Consult [Segment Table Creation](segment-table-creation.md#configure) for more information.
{% endhint %}

#### Optional Envs:

| Attribute | Default |
| :--- | :--- |
| DB\_TYPE | citus |
| DB\_USE\_VIEWS | False |
| DB\_HOST | grnry-pg-citus |
| DB\_USE\_SSL | 5432 |
| PROMETHEUS\_PUSHGATEWAY | "" |
| PROMETHEUS\_JOB | segmentcreation |
| DEBUG | False |
| SOURCE\_SCHEMA\_NAME | public |
| TARGET\_SCHEMA\_NAME | segments |
| SOURCE\_TABLE\_NAME | profilestore |
| COLUMN\_PLACEHOLDER | ? |
| TYPE\_SEPARATOR | :: |
| DEFINITION\_SEPARATOR | = |
| TRANSFORMATION\_SEPARATOR | \| |
| TARGET\_SEGMENT\_INDEX\_SEPARATOR | \| |
| TARGET\_SEGMENT\_VIEW\_SEPARATOR | , |
| TARGET\_SEGMENT\_VIEW\_DEFINITION\_SEPARATOR | : |



#### General Required Envs:

| Attribute |
| :--- |
| TARGET\_SEGMENT\_NAME |
| CITUS\_DIST\_COL |
| SOURCE\_WHERE\_CLAUSE |
| TARGET\_SEGMENT\_VIEWS |
| TARGET\_SEGMENT\_INDEXES |

{% hint style="warning" %}
You now have the option to choose between a pivot or generic transformation. These require slightly different env variables.
{% endhint %}

#### Pivot Type Transformation Envs:

| Attribute |
| :--- |
| TYPE |
| PIVOT\_PATHS |
| PIVOT\_TRANSFORMATIONS |
| PIVOT\_ALLOW\_EMPTY |
| PIVOT\_SEGMENT\_WHERE\_CLAUSE |

#### Generic Type Transformation Envs:

| Attribute |
| :--- |
| TYPE |
| GENERIC\_COLUMNS |
| GENERIC\_TRANSFORMATIONS |

### EnvFromSecrets

EnvFromSecrets are represented as a map where the key is used as a env variable for the Segment Creation Script and the value is [SecretKeyRef](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables) .

The secret is set by the Segment Manager and defaults to `grnry-pg-citus-secret`. If needed, overwrite it with the env variable `DB_SECRET` in the Segment Manager.

{% hint style="info" %}
Consult [Segment Table Creation](segment-table-creation.md#configure) what these values do.
{% endhint %}

#### Example

```text
"envFromSecrets": {
    "DB_USER": {
        "secret": "grnry-secret",
        "key": "superuser-username"
    }
}
```

| Attribute | Default | Required/Optional |
| :--- | :--- | :--- |
| DB\_USER | key: `superuser-username` | Optional |
| DB\_NAME | key: `superuser-database` | Optional |
| DB\_PASSWORD | key: `superuser-password` | Optional |

### Image

{% hint style="info" %}
For information regarding these attributes consult the[ Kubernetes Docs](https://kubernetes.io/docs/concepts/containers/images/). 
{% endhint %}

| Attribute | Default | Required/Optional |
| :--- | :--- | :--- |
| pullPolicy | IfNotPresent | Optional |
| pullSecrets | grnry-base-private-registry-token | Optional |
| repository | gitlab.alvary.io:5000/grnry/segment-table-creation | Optional |
| restartPolicy | OnFailure | Optional |
| tag | 0.7.0 | Optional |



### Further Information

* [https://github.com/operator-framework/operator-sdk](https://github.com/operator-framework/operator-sdk)
* [https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
* [https://gitlab.alvary.io/grnry/segment-manager](https://gitlab.alvary.io/grnry/segment-manager)



