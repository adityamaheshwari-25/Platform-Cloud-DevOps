# Module 9 — Terraform Provisioners

## EC2 recap

Amazon EC2 provides virtual machines called instances. A basic Terraform resource looks like:

```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = "t3.micro"

  tags = {
    Name = "terraform-web"
  }
}
```

Use data sources or a trusted image pipeline rather than permanently hard-coding an image ID that can become obsolete.

## What provisioners do

Provisioners run scripts or transfer files during resource creation or destruction. They are available for special cases, but HashiCorp recommends using them only when no better purpose-built mechanism exists.

## Provisioner types

| Provisioner | Runs where | Purpose |
|---|---|---|
| `local-exec` | Machine running Terraform | Execute a local command |
| `remote-exec` | Newly created remote resource | Run remote commands over SSH or WinRM |
| `file` | Transfers from Terraform runner to remote resource | Copy a file or directory |

## `local-exec`

```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = "t3.micro"

  provisioner "local-exec" {
    command = "printf '%s\n' '${self.public_ip}' >> instance-ips.txt"
  }
}
```

`self` refers to the current resource. Be careful with shell quoting and sensitive values.

## `remote-exec`

```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = "t3.micro"

  connection {
    type        = "ssh"
    user        = var.ssh_user
    private_key = file(var.private_key_path)
    host        = self.public_ip
  }

  provisioner "remote-exec" {
    inline = [
      "sudo systemctl enable nginx",
      "sudo systemctl start nginx"
    ]
  }
}
```

The network, SSH service, credentials, and firewall rules must be ready before the connection can succeed.

## `file` provisioner

```hcl
provisioner "file" {
  source      = "config/app.conf"
  destination = "/tmp/app.conf"
}
```

It uses the resource’s `connection` settings.

## Creation-time and destroy-time behavior

Provisioners run at creation time by default.

Destroy-time example:

```hcl
provisioner "local-exec" {
  when    = destroy
  command = "echo removing ${self.id}"
}
```

Destroy-time provisioners have restrictions and may not run in every failure or removal scenario. Never rely on them as the only cleanup or backup mechanism.

## Failure behavior

By default, a failed creation-time provisioner fails the apply and can leave the resource marked for replacement. `on_failure = continue` ignores the failure, but can hide a broken deployment.

## Better alternatives

Prefer:

- EC2 `user_data` or cloud-init
- Prebuilt images using Packer
- AWS Systems Manager
- Configuration-management tools
- Managed platform services
- Provider-native resource arguments

Provisioners make operations less predictable because Terraform cannot model the full effect of arbitrary scripts.

## Quick check

1. Where does `local-exec` run? **On the Terraform runner.**
2. Which provisioner runs commands over SSH/WinRM? **`remote-exec`.**
3. Should provisioners be the first configuration option? **No; use them as a last resort.**
4. What is often better for initial EC2 bootstrapping? **`user_data`/cloud-init or a prebuilt image.**

## References

- [Terraform provisioners](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
- [Provisioner connection settings](https://developer.hashicorp.com/terraform/language/resources/provisioners/connection)
- [Provisioner alternatives](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax#perform-post-apply-operations)

