---
title: Test data maintenance workflow
source: https://appier.atlassian.net/wiki/spaces/IDASH/pages/4915363968/Test+data+maintenance+workflow
confluence_id: 4915363968
space: IDASH
last_modified: 2026-03-03
author: Angus Hsu
migrated_at: 2026-06-03
---

# Test data maintenance workflow

![Workflow diagram](assets/Wofkflow.png)

## Steps, Links, and Commands

1. Announce the suspension of iTest monitoring in Slack channel **#big-td-platform**

    - iTest heartbeat interval is set to 30 mins ([CAMD-25735](https://appier.atlassian.net/browse/CAMD-25735)). Remember to announce that Opsgenie alerts are expected and can be ignored during the maintenance.

2. Suspend the Argo cron job

    - **Bidder 大禮包**
        - Staging: [itest-regular-test-stg](https://argo-td-alba.appier.us/cron-workflows/idash/itest-regular-test-stg)
        - Production: [itest-regular-test-prd](https://argo-td-alba.appier.us/cron-workflows/idash/itest-regular-test-prd)
    - **AI 大禮包**
        - Production: [itest-regular-test-prd-ai-bidding](https://argo-td-alba.appier.us/cron-workflows/idash/itest-regular-test-prd-ai-bidding)

3. Perform test data maintenance

    - For data addition, create new test data in production iDash then add new cid in iTest config:
        - **Bidder 大禮包** — [idash-api-server-prd-manual-test.yaml](https://github.com/plaxieappier/trading-desk-monitor/blob/master/itest-testcase/idash-api-server-prd-manual-test.yaml) (save as ***testcases.yaml*** in local itest-cli folder)
        - **AI 大禮包** — [ai-bidding-idash-api-server-prd-manual-test.yaml](https://github.com/plaxieappier/trading-desk-monitor/blob/master/itest-testcase/ai-bidding-idash-api-server-prd-manual-test.yaml) (save as ***ai-bidding-testcases.yaml*** in local itest-cli folder)
    - For data modification, update existing test data in production iDash.

4. Update iTest test data & upload to GCS

    Refer to the [README](https://github.com/plaxieappier/itest-cli?tab=readme-ov-file) of the **itest-cli** repo for tool installation, config file preparation, and data upload. Add new cid in local `testcases.yaml` if needed.

    **Optional: install `consul-template` via Homebrew**

    ```bash
    # Tap the HashiCorp repository
    brew tap hashicorp/tap

    # Install Consul Template
    brew install hashicorp/tap/consul-template

    # Verify the installation
    consul-template -version
    ```

5. Resume the Argo cron job.

6. Monitor the successful execution of the Argo cron job.

7. Announce the iTest monitoring is resumed in Slack channel **#big-td-platform**.
