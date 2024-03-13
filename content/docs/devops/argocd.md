---
title: ArgoCD
layout: "layouts/docs.njk"
eleventyNavigation:
  key: ArgoCD
  parent: GitOps 
---

ArgoCD là GitOps Tool mà có nhiệm vụ chính là đảm bảo app state trên Kubernetes đúng với những gì được config trong code lưu trên Git, resource tạo ra trên Kubernetes đúng với file manifest lưu trên Git.

Vấn đề là sau khi tạo resource xong thì để đảm bảo