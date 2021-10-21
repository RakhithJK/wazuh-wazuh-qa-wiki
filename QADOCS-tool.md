## Introduction

`qa-docs` is a tool that allows you to parse documentation blocks within the `wazuh-qa` tests, to index that parsed data to ElasticSearch, and visualize it using a search engine via `search-ui`.

<center><img src="https://github.com/wazuh/wazuh-qa/wiki/images/qadocs_tool_imgs/qa_docs_diagram.png"></center>

When it performs a parse, it creates an internal `output/` directory, which is used to create the index and then visualize it.

## Table of contents

  * [QA-DOCS tool](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool)
    * [Installation guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide)
      * [Supported OS](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#supported-os)
        * [Linux dependencies](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#linux-dependencies)
          * [System dependencies](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#system-dependencies)
        * [Windows dependencies](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#windows-dependencies)
          * [System dependencies](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#system-dependencies-1)
      * [How to install qa-docs](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide#how-to-install-qa-docs)
    * [Use guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-use-guide)
      * [How to use qa-docs tool](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-use-guide#how-to-use-qa-docs-tool)
      * [Parameter restrictions](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-use-guide#parameter-restrictions)
      * [Examples](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-use-guide#examples)
      * [Docker deployment](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-use-guide#docker-deployment)
    * [How to document a test](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test)
      * [Introduction](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#introduction)
      * [Documentation structure](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#documentation-structure)
      * [Schema blocks](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#schema-blocks)
        * [Module block](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#module-block)
        * [Test block](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#test-block)
        * [Remarks](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#remarks)
      * [Pre-defined values](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-How-to-document-a-test#pre-defined-values)

