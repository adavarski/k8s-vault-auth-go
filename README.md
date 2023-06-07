# vault-kubernetes-auth
Demo project that shows how to connect a Kubernetes cluster to an external Hashicorp Vault instance

## Requirements

- Docker
- kubectl
- kind
- skaffold
- terraform

## Run it (2 times)

```sh
$ sh run.sh
```
Example Output:
```
$ sh run.sh 
kind-kind
already using kind cluster
serviceaccount/vault-kubernetes-auth unchanged
clusterrolebinding.rbac.authorization.k8s.io/role-tokenreview-binding unchanged
secret/vault-kubernetes-auth-secret unchanged
deployment.apps/vault unchanged
service/vault unchanged

sleeping for 15s to let kubernetes settle

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/vault from the dependency lock file
- Using previously-installed hashicorp/vault v3.11.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # vault_auth_backend.kubernetes will be created
  + resource "vault_auth_backend" "kubernetes" {
      + accessor        = (known after apply)
      + disable_remount = false
      + id              = (known after apply)
      + path            = (known after apply)
      + tune            = (known after apply)
      + type            = "kubernetes"
    }

  # vault_kubernetes_auth_backend_config.example will be created
  + resource "vault_kubernetes_auth_backend_config" "example" {
      + backend                = (known after apply)
      + disable_iss_validation = true
      + disable_local_ca_jwt   = (known after apply)
      + id                     = (known after apply)
      + kubernetes_ca_cert     = <<-EOT
            -----BEGIN CERTIFICATE-----
            MIIC/jCCAeagAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
            cm5ldGVzMB4XDTIzMDYwNzEyNTExOVoXDTMzMDYwNDEyNTExOVowFTETMBEGA1UE
            AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMEa
            DEUsAF+e3ELJioikEoKwcHqApvkZc/+mGmbEYtYYKaC3gR5s2BzNjcn9YKjTR7A/
            551WAeHtB51+gpuP7sU6aS2IW+Pe4FEK8an2eOoMcz1iTQOLhIchVdpGwCP+qvM/
            HkIyk0LFv1uvw5h1rrBgX/ugNEvL5eaD4TB1dCrGmsFQPYMaIqb+sm7hFHnYZuP9
            UTjfJevnShSGX3PF0axB/VknWTVtPypFMfMnNMUITDfsmSgB/wdmMjiRUEyJOK63
            r1ZbCmZmdl98fa18OqFyaNLJkww6kEM+0Ws6myYdb1k7q1ZvzODkRHGSDTObjs94
            i4Tdc/qDAsbXtwzxJ58CAwEAAaNZMFcwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
            /wQFMAMBAf8wHQYDVR0OBBYEFBGsrm4dyqVuZeQXiLcK3RYXVmAWMBUGA1UdEQQO
            MAyCCmt1YmVybmV0ZXMwDQYJKoZIhvcNAQELBQADggEBACeyENB7NUp7LzYWdU9+
            vkddlRBQegil1IFbDvX5/WjYjSqM4KOMu7zhnTslZuxVzBzNc2iR5ShKxhvjROK+
            ta/yTcWeeb2RiD79yRc9iX78VZG8hr9yv8PYhDMtvdSsmI0A6x3fU1jPDZ3HUczT
            kYN6bEQp/zPiCtBs5WLgiKe3WsjxChGOapf5w1nbFn4lUP/J6kfJK4QnN4dyVgBQ
            xRHKIamVMux3rRq/nWRks6yQWutW3yJo6goz6OjIIwyOXuk7zGdbXqVjoAPBCxL8
            eqaP6v/5UkFgOum/mQldNSuDi2/jdazQxYU4h5fZrhFSUf34qBS6xoHUJY6fUodB
            QhE=
            -----END CERTIFICATE-----
        EOT
      + kubernetes_host        = "https://kubernetes.default.svc:443"
      + token_reviewer_jwt     = (sensitive value)
    }

  # vault_kubernetes_auth_backend_role.example will be created
  + resource "vault_kubernetes_auth_backend_role" "example" {
      + alias_name_source                = (known after apply)
      + audience                         = "https://kubernetes.default.svc.cluster.local"
      + backend                          = (known after apply)
      + bound_service_account_names      = [
          + "*",
        ]
      + bound_service_account_namespaces = [
          + "default",
        ]
      + id                               = (known after apply)
      + role_name                        = "dev-role-k8s"
      + token_policies                   = [
          + "default",
          + "developer",
        ]
      + token_ttl                        = 3600
      + token_type                       = "default"
    }

  # vault_kv_secret_v2.secret will be created
  + resource "vault_kv_secret_v2" "secret" {
      + cas                 = 1
      + data                = (sensitive value)
      + data_json           = (sensitive value)
      + delete_all_versions = true
      + disable_read        = false
      + id                  = (known after apply)
      + metadata            = (known after apply)
      + mount               = "kv"
      + name                = "creds"
      + path                = (known after apply)
    }

  # vault_mount.kvv2 will be created
  + resource "vault_mount" "kvv2" {
      + accessor                     = (known after apply)
      + audit_non_hmac_request_keys  = (known after apply)
      + audit_non_hmac_response_keys = (known after apply)
      + default_lease_ttl_seconds    = (known after apply)
      + description                  = "KV Version 2 secret engine mount"
      + external_entropy_access      = false
      + id                           = (known after apply)
      + max_lease_ttl_seconds        = (known after apply)
      + options                      = {
          + "version" = "2"
        }
      + path                         = "kv"
      + seal_wrap                    = (known after apply)
      + type                         = "kv"
    }

  # vault_policy.bla will be created
  + resource "vault_policy" "bla" {
      + id     = (known after apply)
      + name   = "developer"
      + policy = <<-EOT
            path "kv/*" {
              capabilities = ["read"]
            }
        EOT
    }

Plan: 6 to add, 0 to change, 0 to destroy.
vault_policy.bla: Creating...
vault_mount.kvv2: Creating...
vault_auth_backend.kubernetes: Creating...
vault_policy.bla: Creation complete after 0s [id=developer]
vault_mount.kvv2: Creation complete after 1s [id=kv]
vault_auth_backend.kubernetes: Creation complete after 1s [id=kubernetes]
vault_kubernetes_auth_backend_config.example: Creating...
vault_kv_secret_v2.secret: Creating...
vault_kubernetes_auth_backend_role.example: Creating...
vault_kubernetes_auth_backend_config.example: Creation complete after 0s [id=auth/kubernetes/config]
vault_kv_secret_v2.secret: Creation complete after 1s [id=kv/data/creds]
vault_kubernetes_auth_backend_role.example: Creation complete after 1s [id=auth/kubernetes/role/dev-role-k8s]

Apply complete! Resources: 6 added, 0 changed, 0 destroyed.
/home/davar/Downloads/vault-kubernetes-auth
Generating tags...
 - davarski/vault-kubernetes-auth -> davarski/vault-kubernetes-auth:5a18cd8-dirty
Checking cache...
 - davarski/vault-kubernetes-auth: Not found. Building
Starting build...
Found [kind-kind] context, using local docker daemon.
Building [davarski/vault-kubernetes-auth]...
Target platforms: [linux/amd64]
#2 [internal] load .dockerignore
#2 transferring context: 2B done
#2 DONE 0.1s

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 38B done
#1 DONE 0.1s

#4 [internal] load metadata for docker.io/library/golang:1.19.4
#4 ...

#3 [internal] load metadata for gcr.io/distroless/static:latest
#3 DONE 0.5s

#4 [internal] load metadata for docker.io/library/golang:1.19.4
#4 DONE 1.2s

#5 [final 1/2] FROM gcr.io/distroless/static@sha256:7198a357ff3a8ef750b0413...
#5 DONE 0.0s

#6 [build 1/7] FROM docker.io/library/golang:1.19.4@sha256:660f138b4477001d...
#6 DONE 0.0s

#8 [internal] load build context
#8 transferring context: 30.80MB 0.2s done
#8 DONE 0.2s

#7 [build 2/7] WORKDIR /src
#7 CACHED

#9 [build 3/7] COPY ./go.mod ./go.sum ./
#9 CACHED

#10 [build 4/7] RUN go mod download
#10 CACHED

#11 [build 5/7] COPY ./ ./
#11 DONE 1.6s

#12 [build 6/7] RUN go mod download
#12 DONE 2.0s

#13 [build 7/7] RUN CGO_ENABLED=0 go build -o /vault-kubernetes-auth
#13 DONE 18.7s

#14 [final 2/2] COPY --from=build --chown=nonroot:nonroot /vault-kubernetes-...
#14 CACHED

#15 exporting to image
#15 exporting layers done
#15 writing image sha256:173e1fdf2c9c3f37f9a627b762478c821bd2da419ab3d1ae7e7a4b3b97c2038b
#15 writing image sha256:173e1fdf2c9c3f37f9a627b762478c821bd2da419ab3d1ae7e7a4b3b97c2038b 0.0s done
#15 naming to docker.io/davarski/vault-kubernetes-auth:5a18cd8-dirty done
#15 DONE 0.1s
Build [davarski/vault-kubernetes-auth] succeeded
Starting test...
Tags used in deployment:
 - davarski/vault-kubernetes-auth -> davarski/vault-kubernetes-auth:173e1fdf2c9c3f37f9a627b762478c821bd2da419ab3d1ae7e7a4b3b97c2038b
Starting deploy...
Loading images into kind cluster nodes...
 - davarski/vault-kubernetes-auth:173e1fdf2c9c3f37f9a627b762478c821bd2da419ab3d1ae7e7a4b3b97c2038b -> Loaded
Images loaded in 8.475 seconds
 - deployment.apps/vault-kubernetes-auth created
Waiting for deployments to stabilize...
 - deployment/vault-kubernetes-auth is ready.
Deployments stabilized in 2.143 seconds
You can also run [skaffold run --tail] to get the logs

2023/06/07 12:53:17 Vault auth succeeded!
2023/06/07 12:53:17 Read secret from vault hello=world
^C

```

Ref:
- https://support.hashicorp.com/hc/en-us/articles/4404389946387-Kubernetes-auth-method-Permission-Denied-error
- https://www.twilio.com/blog/manage-go-application-secrets-using-vault
