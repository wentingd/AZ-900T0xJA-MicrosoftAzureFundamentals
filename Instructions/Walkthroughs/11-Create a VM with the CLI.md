---
wts:
    title: '11 - CLIを使用してVMを作成する (10 分)'
    module: 'モジュール 03: コア ソリューションおよび管理ツールに関する説明'
---
# 11 - CLI を使用して VM を作成する

このチュートリアルでは、Azure CLI をローカルにインストールし、リソース グループと仮想マシンを作成し、Cloud Shell を使用し、Azure Advisor の推奨事項を確認します。 

# タスク 1: Cloud Shell を設定する (10 分)

このタスクでは、Cloud Shell を構成します。 

1. [Azure Portal](https://portal.azure.com) にサインインします。

2. Azure Portal の右上にあるアイコンをクリックして、**Azure Cloud Shell** を開きます。

    ![Azure Portal の Azure Cloud Shell アイコンのスクリーンショット。](../images/1002.png)

3. 以前に Cloud Shell を使用したことがある場合は、次のタスクに進みます。 

4. **Bash** と **PowerShell** のいずれかを選択することを求めるプロンプトが表示されたら、**Bash** を選択します。 

5. プロンプトが表示されたら、**ストレージの作成** をクリックし、Azure Cloud Shell が初期化されるまで待ちます。 

# タスク 2: リソースグループと仮想マシンを作成する

このタスクでは、Azure CLI を使用して、リソース グループと仮想マシンを作成します。  

1. 「Cloud Shell」 ペインの左上のドロップダウン メニューで、「**Bash**」 が選択されていることを確認します (選択されていない場合は選択してください)。

    ![Screenshot of Azure Portal の Azure Cloud Shell のスクリーンショット。ドロップダウン メニューで 「Bash」 が強調表示されています。](../images/1002a.png)

2. 「Cloud Shell」 ペイン内の 「Bash」 セッションで、新しいリソース グループを作成します。 

    ```cli
    az group create --name myRGCLI --location EastUS
    ```

3. リソース グループが作成されたことを確認します。

    ```cli
    az group list --output table
    ```

4. 新しい仮想マシンを作成します。このコマンドはすべて 1 行にする必要があります。また、すべてが1行にある場合は、目盛り (`\`) マークを使用しないでください。 

    ```cli
    az vm create \
    --name myVMCLI \
    --resource-group myRGCLI \
    --image UbuntuLTS \
    --location EastUS \
    --admin-username azureuser \
    --admin-password Pa$$w0rd1234
    ```

    > **注記**: Windows コンピューターでコマンド ラインを使用している場合、バックスラッシュ (`\`) 文字をキャレット (`^`) 文字に置き換えてください。
    
    **注記**: コマンドの完了には 2 ～ 3 分かかります。このコマンドは、仮想マシンと、それに関連するストレージ、ネットワーク、セキュリティ リソースなどのさまざまなリソースを作成します。仮想マシンのデプロイが完了するまで、次の手順に進まないでください。 

5. コマンドの実行が終了したら、ブラウザのウィンドウで、「Cloud Shell」 ペインを閉じます。

6. **仮想マシン** を検索し、**myVMCLI** が実行されていることを確認します。

    ![myVMPS が実行中の状態の仮想マシン ページのスクリーンショット。](../images/1101.png)


# タスク 3: Cloud Shell でコマンドを実行する

このタスクでは、Cloud Shell から CLI コマンドを実行する練習を行います。 

1. ポータルから、Azure Portal の右上にある *Azure Cloud Shell のアイコン* をクリックして、 **Azure Cloud Shell** を開きます。

2. **Bash** や **PowerShell** のどちらかを選択するためのプロンプトが表示されたら、**Bash** を選択します。 

3. 名前、リソース グループ、場所、状態など、仮想マシンに関する情報を取得します。PowerState が **実行されている** ことに注目してください。

    ```cli
    az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
    ```

4. 仮想マシンを停止します。仮想マシンの割り当てが解除されるまで請求が続行されることを示すメッセージに注意してください。 

    ```cli
    az vm stop --resource-group myRGCLI --name myVMCLI
    ```

5. 仮想マシンの状態を確認します。これで、PowerState が **停止されます**。

    ```cli
    az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
    ```

# タスク 4: Azure Advisor の推奨事項を確認する

このタスクでは、Azure Advisor の推奨事項を確認します。 

**注記:** 前のラボ（PowerShellでVMを作成する）を実行した場合、このタスクは既に完了しています。 

1. ポータルから、**Advisor** を検索して選択します。 

2. 「アドバイザー」 で、「**概要**」 を選択します。通知の推奨事項は、高可用性、セキュリティ、パフォーマンス、コストごとにグループ化されています。 

    ![アドバイザーの概要ページのスクリーンショット。 ](../images/1103.png)

3. 「**すべての推奨事項**」 を選択し、各推奨事項と推奨されるアクションを表示します。 

    **注記:** リソースに応じて、推奨事項が異なります。 

    ![「アドバイザーすべての推奨事項」 ページのスクリーンショット。 ](../images/1104.png)

4. 推奨事項を CSV または PDF ファイルとしてダウンロードできることに注意してください。 

5. アラートを作成できることに注意してください。 

6. 時間があるので、Azure CLI の実験を続けてください。

お疲れさまでした。PowerShell をローカル コンピュータにインストールし、PowerShell を使用して仮想マシンを作成し、PowerShell コマンドで練習し、Advisor の推奨事項を表示しました。

**注記**: 追加コストを回避するには、このリソース グループを削除します。リソース グループを検索し、リソース グループをクリックして、「**リソース グループの削除**」 をクリックします。リソース グループの名前を確認し、「**削除**」 をクリックします。**通知** を監視して、削除の進行状況を確認します。
