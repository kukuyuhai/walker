---
title: jenkins pipeline 实用脚本
date: 2021-9-18 16:18
tags: jenkins pipeline
---

> 以下为jenkins流水线发布脚本。

```s

pipeline {
    agent any
    
   tools {
        nodejs 'nodejs'
        
   }
    

    stages {
        stage('checkout'){
            steps {
                sh 'rm -rf *'
                git branch: '********', credentialsId: '**********', url: '*******'  
            }
        }  
      
        stage('Build') {
            steps {
                sh """
                    cnpm i 
                    npm run build:stage
                    cd $WORKSPACE/dist
                    tar zcf static.tar.gz *
                """
            }
        } 
        
    }
    post{
        success{
            sshPublisher(publishers: [sshPublisherDesc(configName: 'kaifa', 
            transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'rm -rf /opt/project/{替换成相应文件目录}/*',           		
            execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', 
            remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: ''), sshTransfer(cleanRemote: false, 
            excludes: '', execCommand: 'tar -zxvf /opt/project/{替换成相应文件目录}/static.tar.gz -C /opt/project/{替换成相应文件目录}/', execTimeout: 1200000, flatten: false, makeEmptyDirs: false, 
            noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '{替换成相应文件目录}', remoteDirectorySDF: false, 
            removePrefix: 'dist/', sourceFiles: 'dist/static.tar.gz', usePty: true)], usePromotionTimestamp: false, 
            useWorkspaceInPromotion: false, verbose: true)])
            
       }
    }
}
```