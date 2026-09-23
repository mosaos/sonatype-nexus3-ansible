# Creating a Repository with Nexus Repository Manager

Nexus Repository Manager (hereinafter NXRM) is an application for managing repositories that supports multiple formats. The Community edition is available for free.

This document describes how to create a Docker repository using NXRM.

## References

- [Try Docker Registry with Nexus Repository Manager 3](https://qiita.com/fukasawah/items/48330564736d3368b632)
- [Create a Private Docker Repository with Nexus](https://qiita.com/Imasug/items/71d2ff21c7d1d3454bff)

## System Requirements

- OS: CentOS 8
- CPU: 4 cores or more
- Memory: 4 GB or more (The System Requirements below says 8 GB, but it worked somehow with 4 GB.)
- Disk: 20 GB or more

https://help.sonatype.com/repomanager3/installation/system-requirements

If there is not enough memory or disk space, NXRM may fail to start without even outputting an error log. Be careful.

## Installation

An Ansible playbook was created separately, so use it to install NXRM.

## Accessing NXRM

Access NXRM by opening:

```text
http://<installation-host>/
```

The password for the administrator account is stored in the following file. Use this password to log in.

```text
/opt/sonatype-work/nexus3/admin.password
```

## Allow Anonymous Access

When the Repository Manager screen is displayed, click [Sign in] in the upper right and log in using the password mentioned above.

When logging in for the first time, you will be asked to reset the password. Reset the password for the `admin` account as appropriate.

After this, you will be asked whether to allow anonymous access. Decide whether to allow it based on your operational policy.

## Repositories

See [Formats - Nexus Repository Manager 3](https://help.sonatype.com/repomanager3/formats) for the repository formats that can be used.

Before creating a repository, some points to consider in NXRM are described below.

### Storage

### Repository Types

There are three types of repositories:

- **proxy**  
  Works as a proxy for an external repository. For example, when accessing Docker Hub or Maven Central Repository from inside a company, creating a proxy repository can reduce traffic and latency when retrieving data.

- **group**  
  Combines multiple repositories. By combining proxy and hosted repositories, and using hosted for registering repositories from inside the company and group for references, resources in external repositories and resources in your own repositories can be accessed transparently.

- **hosted**  
  Creates your own repository. Use hosted when creating a repository for registering and using your own libraries.

## Creating a Docker Repository

### Repository

1. Click the gear icon (`Server administration and configuration`) in the top menu.
2. The administration screen will be displayed. Click [`Repositories`] in the left menu.
3. The repository list will be displayed. Click [`Create Repository`].
4. Select `docker (hosted)`.
5. The settings screen will be displayed. Enter the following:
   - `Name`: An appropriate identifier
   - `Online`: Check it
   - `Enable Docker V1 API`: Check it  
     when compatibility with older Docker clients or tools is required. For internal self-hosted environments, enabling it may be preferable for compatibility. For externally accessible environments, consider whether it is necessary.

   Leave the other settings as default.

6. After configuring the settings, click the [`Create repository`] button.

- For a guide on whether to use SSL, see [SSL and Repository Connector Configuration](https://help.sonatype.com/repomanager3/formats/docker-registry/ssl-and-repository-connector-configuration).
- SSL should be used, but instead of establishing the SSL connection directly with NXRM, HTTPS is handled by a proxy (nginx) in front of NXRM as necessary.
