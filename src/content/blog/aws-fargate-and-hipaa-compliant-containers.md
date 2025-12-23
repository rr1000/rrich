---
title: "AWS Fargate and HIPAA Compliant Containers"
date: 2017-01-01
description: "Initial thoughts on AWS Fargate and its HIPAA compliance implications."
---

The third day of AWS re:Invent was kicked off this morning with Andy Jassy, CEO of Amazon Web Services, announcing a slew of <a href="https://aws.amazon.com/new/reinvent/">new AWS services</a>. One such service that has us excited is Fargate.

As Randall Hunt put it in a <a href="https://aws.amazon.com/blogs/aws/aws-fargate/">company blog post</a> announcing the new service:

<blockquote> <p>AWS Fargate is an easy way to deploy your containers on AWS. To put it simply, Fargate is like EC2 but instead of giving you a virtual machine you get a container. It's a technology that allows you to use containers as a fundamental compute primitive without having to manage the underlying instances."</p>

</blockquote><p>We've seen an incredible increase in container adoption over the last couple of years. Datica has been a huge proponent of containerization technology since our founding. We use Docker in production on a daily basis to deliver HIPAA compliant environments to thousands of users. We even built our own orchestration layer in 2014, one year before <a href="https://cloudplatform.googleblog.com/2015/07/Kubernetes-V1-Released.html">Kubernetes 1.0</a> was released. To say we're bought into the concept of containers would be a massive understatement.</p>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">No longer any need to manage servers and clusters for your containers - AWS Fargate - Run ECS and EKS without managing servers <a href="https://twitter.com/hashtag/reInvent?src=hash&amp;ref_src=twsrc%5Etfw">#reInvent</a> <a href="https://t.co/oPsXdvrSAL">https://t.co/oPsXdvrSAL</a> <a href="https://t.co/lZnS558CXV">pic.twitter.com/lZnS558CXV</a></p>&mdash; AWS re:Invent (@AWSreInvent) <a href="https://twitter.com/AWSreInvent/status/935910336212844544?ref_src=twsrc%5Etfw">November 29, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<p>It's because of our long history of containerization support that we're excited about AWS Fargate. However, that excitement isn't without reservation. As is the case with most new technology, compliance and security pose great barriers to adoption. We're deeply aware of these barriers at Datica. <a href="https://datica.com/platform/">The Datica Platform</a> was designed to address the complexities of compliance and security specific to the healthcare industry. The fact is, understanding compliance is difficult enough. That difficulty is exasperated exponentially when it comes time to not only implement that understanding through policy, but prove compliance beyond a reasonable doubt to auditors.</p>

<p>Our primary reservation with Fargate is that it currently is not eligible for <a href="https://datica.com/discover/hipaa-compliance/">HIPAA compliance</a>. Because we <a href="https://datica.com/blog/3-common-misconceptions-about-business-associate-agreements/">sign a BAA</a> with both AWS and our customers, we cannot guarantee full-stack compliance against a service that is not scoped within the BAA that we sign with AWS. This means our customers in the healthcare industry cannot take advantage of such a service.</p>

<p>Luckily, we've been working on a Docker-based deployment feature for the better part of the last quarter. By working directly with our customers on improving our deployment process, we're approaching a limited release of Datica BYOD (bring your own docker). BYOD will give users the ability to deploy docker containers directly on the platform. No longer will deployments be restricted to the <code>git push</code> model. In the future—once it becomes HIPAA eligible—we plan to take complete advantage of AWS Fargate. Until then, we're confident BYOD will provide an excellent deployment experience for our customers.</p>

