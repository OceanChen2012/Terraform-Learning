# Terraform-Learning
![图片](https://user-images.githubusercontent.com/2436384/157817973-a37ecd2d-9f00-4b25-8c39-bf8394d0cd27.png)

![图片](https://user-images.githubusercontent.com/2436384/157819952-2d7a2593-fd83-4a0d-abcd-35ce2b27e0ec.png)

![图片](https://user-images.githubusercontent.com/2436384/157819997-92358c38-3670-4be8-9c99-3ef320fe9251.png)


多云基础设施编排工具Terraform,重点解决公有云厂商的软硬件（基础）资源构建的问题。
公司最近用上了terraform, 新建服务, 更改配置, 增加SG都要通过他进行.

terraform是个什么东西呢? 想一下, 其实就是在各个云平台提供的API只上, 搞出来一个抽象层.

Terraform的生态环境到了今天，已经发展为三个分支，分别是：

    1）开源版
    2）Terraform Cloud云服务版
    3）Terraform企业版
	
	

Terraform基础概念：（1）Provider （2）状态管理

Terraform基础概念——Provider


Terraform被设计成一个多云基础设施编排工具，不像CloudFormation那样绑定AWS平台，Terraform可以同时编排各种云平台或是其他基础设施的资源。Terraform实现多云编排的方法就是Provider插件机制。
Terraform使用的是HashiCorp自研的go-plugin库(https://github.com/hashicorp/go-plugin)，本质上各个Provider插件都是独立的进程，与Terraform进程之间通过rpc进行调用。Terraform引擎首先读取并分析用户编写的Terraform代码，形成一个由data与resource组成的图(Graph)，再通过rpc调用这些data与resource所对应的Provider插件；Provider插件的编写者根据Terraform所制定的插件框架来定义各种data和resource，并实现相应的CRUD方法；在实现这些CRUD方法时，可以调用目标平台提供的SDK，或是直接通过调用Http(s) API来操作目标平台。


Provider的声明

一组Terraform代码要被执行，相关的Provider必须在代码中被声明。不少的Provider在声明时需要传入一些关键信息才能被使用，例如我们在第一章的例子中，必须给出访问密钥以及期望执行的UCloud区域（Region）信息。
terraform {
  required_providers {
    ucloud    = {
      source  = "ucloud/ucloud"
      version = ">=1.24.1"
    }
  }
}

provider "ucloud" {
  public_key  = "your_public_key"
  private_key = "your_private_key"
  project_id  = "your_project_id"
  region      = "cn-bj2"
}



Terraform基础概念——状态管理


Terraform引入了一个独特的概念——状态管理，这是Ansible等配置管理工具或是自研工具调用SDK操作基础设施的方案所没有的。简单来说，Terraform将每次执行基础设施变更操作时的状态信息保存在一个状态文件中，默认情况下会保存在当前工作目录下的terraform.tfstate文件里。例如我们在代码中声明一个data和一个resource：
data "ucloud_images" "default" {
  availability_zone = "cn-sh2-01"
  name_regex        = "^CentOS 6.5 64"
  image_type        = "base"
}

resource "ucloud_vpc" "vpc" {
  cidr_blocks = ["10.0.0.0/16"]
  name = "my-vpc"
}

基本概念
Terraform是一个云上资源编排工具，实现基础设施即代码。

    Terraform能够让您在公有云上轻松使用简单模板语言定义、预览和部署云基础架构。更多信息，请参见Configuration Syntax。
    Terraform是一个安全、高效地部署、更改、版本化基础设施和应用程序的工具，可以用来管理多层次的资源。
    Terraform统一管理从上层的软件到底层的网络、系统的配置。
    Terraform可以创建、修改、删除ECS、VPC、RDS、SLB等多种公有云云产品资源。

使用优势

    将基础架构部署到多个云

    Terraform适用于多云方案，将类似的基础架构部署到阿里云、其他云提供商或者本地数据中心。开发人员能够使用相同的工具和相似的配置文件同时管理不同云提供商的资源。
    自动化管理基础架构

    Terraform能够创建配置文件的模板，以可重复、可预测的方式定义、预配和配置ECS资源，减少因人为因素导致的部署和管理错误。通过使用Terraform，您能够多次部署同一模板，创建相同的开发、测试和生产环境。
    基础架构即代码

    可以用代码来管理维护资源。允许保存基础设施状态，从而使您能够跟踪对系统（基础设施即代码）中不同组件所做的更改，并与其他人共享这些配置。
    降低开发成本

    您可通过按需创建开发和部署环境来降低成本。并且，您可以在系统更改之前进行评估。




更多资料参考：
http://registry.terraform.io/browse/providers
https://lonegunmanb.github.io/introduction-terraform/
