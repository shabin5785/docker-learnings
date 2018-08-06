# docker-learnings

- Containers are consistent across different devices. It runs the same way fundamentaly in any device. So we dont need to worry about the way software runs in different platforms

- migrating app to container doesnt need any code change. It runs the same way as before, but in containers. 

- mac natively doesnt support docker, same as win10. So a small virtual os needs to be run in these os to run docker in them ( dont use brew for docker in mac). So mac/win are similar to this feature.`

- In docker edge version means beta version. Its supported only for a month, after that a new version is realeased.  Stable is supported for around 4 months, after that a new one is released.

- in Windows we can have a linux container or a windows container. windows container is a newer one as MS started supporting it recently. Not all version of docker work for all versions of windows.Old version of windows needs to use legacy docker called as Dokcer tool box. Same with old mac os versions. Dont install docker from linux packages as these are old.  Windows is going to support native linux containers soon. 

- Newer versions of windows in background docker for win is using Hyper-V wiht tiny linux VM for linux containers. This does not work well with virtualbox. as hyper-v doesnt work with virtualbox. Old versions of windows uses a docker-machine which is a linux vm in virtual box, these two doesnt support windows containers. Windows server 2016 support native windows containers. 
