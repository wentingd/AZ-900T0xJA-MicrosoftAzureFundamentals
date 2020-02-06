---
wts:
    title: '10 - PowerShell を使用して VM を作成する'
    module: 'モジュール 02 - コア Azure サービス'
---
# 10 - PowerShell を使用して VM を作成する

このチュートリアルでは、PowerShell をローカルにインストールし、リソース グループと仮想マシンを作成し、Cloud Shell にアクセスして使用し、Azure Advisor の推奨事項を確認します。 

推定時間: 35 分

# タスク 1: PowerShell をローカルで構成する

このタスクでは、ローカル コンピューターから実行するように PowerShell を構成します。 

1. ローカル コンピューターで、タスク バーから [**スタート**] アイコンを選択します。**PowerShell** と入力して、**Windows PowerShell App** を見つけます。アプリを右クリックして、[**管理者として実行**] を選択します。プロンプトが表示されたら、**はい** と答え、アプリを信頼することを表明します。 

    **注記:** Linux および macOS の場合、次のコマンドを使用して、昇格した特権で PowerShell Core を起動できます。

```bash
sudo pwsh
```

2. PowerShell プロンプトで、Azure PowerShell モジュールをインストールします。プロンプトが表示されたら、 **すべてにはい** と答えてリポジトリを信頼することを表明します。インストールが完了するまで数分かかる場合があります。

```PowerShell
Install-Module Az -AllowClobber
```

    **注記**: Windows ユーザーは *NuGet* プロバイダーのインストールに同意し、プロンプトが表示された場合は *PowerShell ギャラリー* (PSGallery) からのモジュールのインストールに同意する必要があります。スクリプト実行の失敗を受け取った場合は、昇格された PowerShell セッションで `Set-ExecutionPolicy RemoteSigned` を実行します。 

3. 最新の Az モジュールの更新プログラムを取得します。 

```PowerShell
Update-Module -Name Az
```

    **注記:** プロンプトが表示されたら、**すべてにはい** と答え、Az モジュールへの更新を信頼することを表明します。Az モジュールの最新バージョンが既にインストールされている場合は、プロンプトが自動的に返されます。

# タスク 2: リソース グループと仮想マシンを作成します。

このタスクでは、PowerShell を使用して、リソース グループと仮想マシンを作成します。  

1. ローカル コンピューターから Azure に接続し、プロンプトが表示されたら、Azure ログイン資格情報を入力します。返されるサブスクリプションとアカウント情報を確認します。 

```PowerShell
Connect-AzAccount
```

2. 新しいリソース グループを作成します。 

```PowerShell
New-AzResourceGroup -Name myRGPS -Location EastUS
```

3. 新しいリソース グループを確認します。 

```PowerShell
Get-AzResourceGroup | Format-Table
```

4. 仮想マシンを作成するプロンプトが表示されたら、新しいマシンの名前 (**myVMPS**)、ユーザー名 (**azureuser**)、パスワード (**Pa$$w0rd1234**) を指定します。コマンドが 1 行で入力されていることを確認します。また、すべてが1行にある場合は、目盛り (`) マークを使用しないでください。 

```PowerShell
    New-AzVm `
    -ResourceGroupName "myRGPS" `
    -Name "myVMPS" `
    -Location "East US" `
    -VirtualNetworkName "myVnetPS" `
    -SubnetName "mySubnetPS" `
    -SecurityGroupName "myNSGPS" `
    -PublicIpAddressName "myPublicIpPS" `
```

5. [Azure Portal](https://portal.azure.com) にサインインします。

6. **仮想マシン**を検索し、**myVMPS** が実行されていることを確認します。これには数分かかる場合があります。

    ![myVMPS が実行中の状態の仮想マシン ページのスクリーンショット。](../images/1001.png)

7. 新しい仮想マシンにアクセスし、[概要] と [ネットワーク] の設定を確認して、情報が正しくデプロイされたことを確認します。 

8. ローカルの PowerShell セッションを閉じます。 

# タスク 3: Cloud Shell でコマンドを実行する

このタスクでは、Cloud Shell から PowerShell コマンドを実行する練習を行います。 

1. ポータルから、Azure Portal の右上にあるアイコンをクリックして、**Azure Cloud Shell** を開きます。

    ![Azure Portal Azure Cloud Shell アイコンのスクリーンショット。](../images/1002.png)

2. 以前に Cloud Shell を使用したことがある場合は、手順 5 に進みます。 

3. **Bash** や **PowerShell** のどちらかを選択するためのプロンプトが表示されたら、**PowerShell** を選択します。 

4. プロンプトが表示されたら、**ストレージを作成** し、Azure Cloud Shell の初期化を許可します。 

5. 左上のドロップダウンメニューで **PowerShell** が選択されていることを確認します。

6. 名前、リソース グループ、場所、状態など、仮想マシンに関する情報を取得します。PowerState が **実行されている** ことに注目してください。 

```PowerShell
Get-AzVM -name myVMPS -status | Format-Table -autosize
```

7. 仮想マシンを停止します。プロンプトが表示されたら、アクションを確認 (はい) します。 

```PowerShell
Stop-AzVM -ResourceGroupName myRGPS -Name myVMPS
```

8. 仮想マシンの状態を確認します。これで、PowerState の **割り当てが解除されます**。ポータルで仮想マシンの状態を確認することもできます。 

```PowerShell
Get-AzVM -name myVMPS -status | Format-Table -autosize
```

# タスク 4: Azure Advisor の推奨事項を確認する

**注記:** この同じタスクは、「Azure CLI を使用した VM の作成」にあります。 

このタスクでは、仮想マシンの Azure Advisor の推奨事項を確認します。 

1. ポータルから、**Advisor** を検索して選択します。 

2. [アドバイザー] で、[**概要**] を選択します。通知の推奨事項は、高可用性、セキュリティ、パフォーマンス、コストごとにグループ化されています。 

    ![アドバイザーの概要ページのスクリーンショット。 ](../images/1003.png)

3. [**すべての推奨事項**] を選択し、各推奨事項と推奨されるアクションを表示します。  

    **注記:** リソースに応じて、推奨事項が異なります。 

    ![[アドバイザーすべての推奨事項] ページのスクリーンショット。 ](../images/1004.png)

4. 推奨事項を CSV または PDF ファイルとしてダウンロードできることに注意してください。 

5. アラートを作成できることに注意してください。 

6. 時間があるので、Azure PowerShell の実験を続けてください。 

お疲れさまでした。PowerShell をローカル コンピュータにインストールし、PowerShell を使用して仮想マシンを作成し、PowerShell コマンドで練習し、Advisor の推奨事項を表示しました。

**注記**: 追加コストを回避するには、このリソース グループを削除します。リソース グループを検索し、リソース グループをクリックして、[**リソース グループの削除**] をクリックします。リソース グループの名前を確認し、[**削除**] をクリックします。**通知** を監視して、削除の進行状況を確認します。