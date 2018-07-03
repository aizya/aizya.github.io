---
layout: post
title: Restful中@PathParam与@QueryParam之间的区别
date: 2017-08-15
categories: Restful
tags: [Restful]
description: 简单的了解一下Restful中@PathParam与@QueryParam的区别
published: true
---

### What is the difference between @PathParam and @QueryParam?

Query parameters are added to the url after the ? mark, while a path parameter is part of the regular URL.

1. 使用@PathParam修饰的参数,直白点说,会被直接加入路径当中去,以下面的类作为介绍:

        @Path("/projects")
        public interface ProjectRestService {
            @GET
            @Produces(MediaType.APPLICATION_JSON)
            public GeneralResponse<ProjectBean> getAllProjects();
            
            @GET
            @Path("/{projectName}")
            @Produces({ MediaType.APPLICATION_JSON })
            public GeneralResponse<ProjectBean> getProject(@PathParam("projectName") String projectName);
        }

**如果要访问getProject()方法,在服务器启动的时候,可以在(Restlet Client)直接以localhost:8080/rest/projects/{projectName}进行获取,其中大括号中的是需要用户进行手动输入的.**

Tip: Restlet Client 是Chromium浏览器的一个Extra Extension,可以搜索添加...

2. 使用@QueryParam修饰的参数,将会直接被作为请求的参数,以下面这个类作为介绍:

        @Path("/projectfiles")
        public interface ProjectFileRestService {
            
            @GET
            @Produces(MediaType.APPLICATION_JSON)
            public GeneralResponse<ProjectFileBean> getAllProjectFiles();
            
            @GET
            //@Path("/{fileNo}/{projectName}/{userName}")
            @Produces({ MediaType.TEXT_PLAIN })
            public Response downloadProjectFile(@QueryParam("fileNo") String fileNo,
                    @QueryParam("projectName") String projectName, @QueryParam("userName") String userName);
        }

**同理,如果想要对上面的方法进行访问的话,可以使用localhost:8080/projectfiles?fileNo={fileNo}&projectName={projectName}&userName={userName}的方式进行访问...注意,在大括号中填写需要查找的准确内容,在这里表示的就是,输入了正确信息,就可以直接将项目中的某个文件下载下来...**

二者之间的共同点,都会出现在请求的URL中,但是不同的是,一个是作为请求的部分路径(@PathParam),而另一个则是作为请求的请求参数(@QueryParam).而一旦参数被@QueryParam修饰之后,就不需要在@Path注解当中进行声明,而是直接在调用的时候进行填写,其实是无二的..

在日常的开发过程中,我们ADD的原则就是规避在一个方法有多个参数的时候,使用@PathParam.这主要是为了避免层级过深的URI。/在url中表达层级，过深的导航容易导致url膨胀，不易维护. 因此,一般多个参数的时候,都使用@QueryParam进行表示...

