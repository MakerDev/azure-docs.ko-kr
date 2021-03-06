---
title: HDInsight에서 Apache Hadoop 작업용 데이터 업로드
description: HDInsight에서 Apache Hadoop 작업에 대 한 데이터를 업로드 및 액세스 하는 방법에 대해 알아봅니다. Azure 클래식 CLI, Azure Storage 탐색기, Azure PowerShell, Hadoop 명령줄 또는 Sqoop을 사용 합니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdiseo17may2017,seoapr2020
ms.date: 04/27/2020
ms.openlocfilehash: d642397ef42227e530bd9eff14c3da6078241281
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085874"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>HDInsight에서 Apache Hadoop 작업용 데이터 업로드

HDInsight는 Azure Storage 및 Azure Data Lake Storage에 대 한 HDFS (Hadoop 분산 파일 시스템)를 제공 합니다. 이 저장소는 Gen1 및 Gen2를 포함 합니다. Azure Storage 및 Data Lake Storage Gen1 및 Gen2는 HDFS 확장으로 설계 되었습니다. 이를 통해 Hadoop 환경에서 구성 요소의 전체 집합이 관리 하는 데이터에 대해 직접 작동할 수 있습니다. Azure Storage, Data Lake Storage Gen1 및 Gen2는 별개의 파일 시스템입니다. 시스템은 데이터를 저장 하 고 해당 데이터를 계산 하는 데 최적화 되어 있습니다. Azure Storage 사용의 이점에 대 한 자세한 내용은 [HDInsight에서 Azure Storage 사용](hdinsight-hadoop-use-blob-storage.md)을 참조 하세요. 또한 HDInsight에서 [Data Lake Storage Gen1 사용](hdinsight-hadoop-use-data-lake-store.md)및 [Hdinsight에서 Data Lake Storage Gen2 사용](hdinsight-hadoop-use-data-lake-storage-gen2.md)을 참조 하세요.

## <a name="prerequisites"></a>전제 조건

시작하기 전에 다음 요구 사항을 확인하세요.

* Azure HDInsight 클러스터를 만듭니다. 자세한 지침은 [Azure HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)을 참조 하세요.
* 다음 문서의 정보:
    * [HDInsight에서 Azure Storage 사용](hdinsight-hadoop-use-blob-storage.md)
    * [HDInsight에서 Data Lake Store Gen1 사용](hdinsight-hadoop-use-data-lake-store.md)
    * [HDInsight에서 Data Lake Store Gen2 사용](hdinsight-hadoop-use-data-lake-storage-gen2.md)  

## <a name="upload-data-to-azure-storage"></a>Azure Storage에 데이터 업로드

### <a name="utilities"></a>공공 시설

Microsoft는 Azure Storage에서 작업할 다음 유틸리티를 제공합니다.

| 도구 | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AZCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |
| [Hadoop 명령](#hadoop-command-line) |✔ |✔ |✔ |

> [!NOTE]  
> Hadoop 명령은 HDInsight 클러스터에서만 사용할 수 있습니다. 이 명령을 통해 로컬 파일 시스템의 데이터를 Azure Storage로 로드할 수만 있습니다.  

### <a name="hadoop-command-line"></a>Hadoop 명령줄

데이터가 클러스터 헤드 노드에 존재하는 경우 Hadoop 명령줄은 Azure Storage Blob에 데이터를 저장하는데만 유용합니다.

Hadoop 명령을 사용 하려면 먼저 [SSH 또는 PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md)를 사용 하 여 헤드 노드에 연결 해야 합니다.

연결된 후에 스토리지에 파일을 업로드하려면 다음 구문을 사용할 수 있습니다.

```bash
hadoop fs -copyFromLocal <localFilePath> <storageFilePath>
```

예를 들면 `hadoop fs -copyFromLocal data.txt /example/data/data.txt`과 같습니다.

HDInsight에 대 한 기본 파일 시스템은 Azure Storage 이므로/example/data/data.txt 실제로는 Azure Storage에 있습니다. 파일이 다음과 같을 수도 있습니다.

`wasbs:///example/data/data.txt`

또는

`wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt`

파일에 사용 되는 다른 Hadoop 명령 목록은 다음을 참조 하세요.[https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> Apache HBase 클러스터에서 데이터 쓰기 시 사용되는 기본 블록 크기는 256KB입니다. HBase API 또는 REST API를 사용할 때는 잘 작동하는 반면 `hadoop` 또는 `hdfs dfs` 명령을 사용하여 12GB를 초과하는 데이터를 기록하면 오류가 발생합니다. 자세한 내용은 [blob에서 쓰기에 대 한 저장소 예외](hdinsight-troubleshoot-hdfs.md#storage-exception-for-write-on-blob)를 참조 하세요.

### <a name="graphical-clients"></a>그래픽 클라이언트

Azure Storage를 사용하기 위한 그래픽 인터페이스를 제공하는 몇몇 애플리케이션이 있습니다. 다음 테이블은 이러한 애플리케이션 중 일부의 목록입니다.

| 클라이언트 | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [HDInsight 용 Microsoft Visual Studio Tools](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) |✔ |✔ |✔ |
| [`Cerulea`](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](https://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Microsoft Azure용 CloudBerry Explorer](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

## <a name="mount-azure-storage-as-local-drive"></a>Azure Storage를 로컬 드라이브로 탑재

[Azure Storage를 로컬 드라이브로 탑재](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)를 참조하세요.

## <a name="upload-using-services"></a>서비스를 사용하여 업로드

### <a name="azure-data-factory"></a>Azure 데이터 팩터리

Azure Data Factory 서비스는 데이터를 작성 하기 위한 완전히 관리 되는 서비스로, 저장소, 처리 및 이동 서비스를 간소화 되 고, 조정 가능 하며, 신뢰할 수 있는 데이터 프로덕션 파이프라인으로 만듭니다.

|스토리지 유형|설명서|
|----|----|
|Azure Blob 스토리지|[Azure Data Factory를 사용하여 Azure Blob Storage에(서) 데이터 복사](../data-factory/connector-azure-blob-storage.md)|
|Azure Data Lake Storage Gen1|[Azure Data Factory를 사용하여 Azure Data Lake Storage Gen1 간에 데이터 복사](../data-factory/connector-azure-data-lake-store.md)|
|Azure Data Lake Storage Gen2 |[Azure Data Factory를 사용하여 Azure Data Lake Storage Gen2에 데이터 로드](../data-factory/load-azure-data-lake-storage-gen2.md)|

### <a name="apache-sqoop"></a>Apache Sqoop

Sqoop은 Hadoop과 관계형 데이터베이스 간 데이터 전송을 위해 설계된 도구입니다. 이를 사용 하 여 SQL Server, MySQL 또는 Oracle과 같은 관계형 데이터베이스 관리 시스템 (RDBMS)에서 데이터를 가져옵니다. 그런 다음 HDFS (Hadoop distributed file system)로 전환 합니다. MapReduce 또는 Hive를 사용 하 여 Hadoop의 데이터를 변환한 다음 데이터를 RDBMS로 다시 내보냅니다.

자세한 내용은 [HDInsight에서 Sqoop 사용](hadoop/hdinsight-use-sqoop.md)을 참조하세요.

### <a name="development-sdks"></a>개발 SDK

Azure Storage는 다음 프로그래밍 언어에서 Azure SDK를 사용하여 액세스할 수 있습니다.

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Azure SDK 설치에 대한 자세한 내용은 [Azure 다운로드](https://azure.microsoft.com/downloads/)

## <a name="next-steps"></a>다음 단계

이제 HDInsight로 데이터를 가져오는 방법을 이해 했으므로 다음 문서를 참조 하 여 분석에 대해 알아보세요.

* [Azure HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [프로그래밍 방식으로 Apache Hadoop 작업 제출](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight에서 Apache Hive 사용](hadoop/hdinsight-use-hive.md)
