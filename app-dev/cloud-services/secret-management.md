# Secret Management

## Cloud Secret Manager

Secret Manager is a secure and convenient storage system for API keys, passwords, certificates, and other sensitive data. Secret Manager provides a central place and single source of truth to manage, access, and audit secrets across Google Cloud.

### Enable API

```bash
gcloud services enable secretmanager.googleapis.com
```

### Create a Secret

```bash
echo "qwerty" | \
  gcloud secrets create order-db-password --data-file=- --replication-policy=automatic
```

### List Secrets

```bash
gcloud secrets list
```

### Delete a Secret

```bash
gcloud secrets delete order-db-password
```

### Assign IAM Permission

You can finely control CRUD permissions for an account \(user account, service account, a Google Group\) to a secret.  See the [Secret Manager IAM access control](https://cloud.google.com/secret-manager/docs/access-control) for more information. 

```bash
gcloud secrets add-iam-policy-binding --help
```

## Spring Cloud Secret Manager

You can easily get value from Secret Manager by using [Spring Cloud GCP's Secret Manager starter](https://cloud.spring.io/spring-cloud-static/spring-cloud-gcp/1.2.3.RELEASE/reference/html/#secret-manager).

### Dependency

Add the Spring Cloud GCP Secret Manager starter:

{% tabs %}
{% tab title="Maven" %}
```bash
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-gcp-starter-secretmanager</artifactId>
</dependency>
```
{% endtab %}

{% tab title="Gradle" %}
```bash

compile group: 'org.springframework.cloud', name: 'spring-cloud-gcp-starter-secretmanager'
```
{% endtab %}
{% endtabs %}

### Configuration

Secret Manager can be configured during Bootstrap phase, via `bootstrap.properties`. The starter automatically enables Secret Manager integration. But you can also disable it by configuring `spring.cloud.gcp.secretmanager.enabled=false` in a different Spring Boot profile.

{% hint style="info" %}
Read [Spring Cloud GCP Secret Manager configuration](https://cloud.spring.io/spring-cloud-static/spring-cloud-gcp/1.2.3.RELEASE/reference/html/#configuration-10) documentation for more details.
{% endhint %}

### Property Source

You can access individual secrets in stored in Secret Manager by looking up property keys with the `sm://` prefix.

#### @Value Annotation

You can inject the secret value by using the `Value` annotation.

```java
@Value("sm://order-db-password") String databasePassword;
```

#### Properties Mapping

You can refer to the secret value like any other properties, and reference the secret values in a `properties` file.

```text
spring.datasource.password=${sm://order-db-password}
```

#### Property Key Syntax

| Form | Example |
| :--- | :--- |
| Short | `sm://order-db-password` |
| Short - Versioned | `sm://order-db-password/1` |
| Short - Project Scoped and Versioned | `sm://your-project/order-db-password/1` |
| Long - Project Scoped | `sm://projects/your-project/order-db-password/1` |
| Long - Fully Qualified | `sm://projects/your-project/secrets/order-db-password/versions/1` |
