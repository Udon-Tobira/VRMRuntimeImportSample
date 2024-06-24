# VRMImportSample

VRMをエディター時と、ゲーム時の両方でインポートできるようにするサンプルプロジェクト。
[VRM4Uプラグイン](https://ruyo.github.io/VRM4U/01_quick-start/)を使用しています。

## エディター使用時にインポートして、操作可能なサードパーソンキャラクターとして設定する方法

### メッシュのインポート
1. プロジェクトをサードパーソンテンプレートで作成するか、以下必要なものを随時移行してくる。
2. VRM のインポートをできるようにするプラグイン(VRM4U)のダウンロードをする。  
[VRM4U-セットアップ](https://ruyo.github.io/VRM4U/01_quick-start/)
3. プロジェクトを開き、コンテンツフォルダにVRMファイルをドラッグ&ドロップ。(なお、インポートすると大量のファイルが追加されるので、事前にモデル専用のフォルダを用意しておくと良い、またこの後アニメーションもアセットが増えるので、アニメーション用のフォルダもあると良い)  
例：Content/(モデル名)/Mesh と Content/(モデル名)/Animations を作っておく
4. インポートの設定画面では、Material Type を MToon Lit に設定してインポート。
5. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Mesh > Skeletal Mesh Asset にインポートしたスケルタルメッシュを設定。
6. これで起動すると、そのキャラで動かせるが、アニメーションを適切に設定していないので、T字のまま平行移動することになる。以降でアニメーションの設定をしていく。

### アニメーションのリターゲティング
- 特定のスケルタルメッシュ用に作られたアニメーションを、別のスケルタルメッシュに適用することを「リターゲティング」という。
- 今回は、サードパーソンのマネキン用に作られたアニメーションを流用するので、リターゲティングが必要。
1. サードパーソンテンプレートにある RTG_Mannequin ファイル(IK Retargeter 型)を開く。
2. Target IKRig Asset に IK_(モデル名)_Mannequin をセットする。
3. するとこのように左右に比較表示されるのだが、恐らく左と右でポーズが違うと思う。左をAポーズ、右をTポーズという。このポーズを揃えないと適切にアニメーションが設定されない。
    <details>
    <summary>ポーズを揃えるには</summary>

    1. 「Running Retarget」をクリックして、「Editing Retarget Pose」モードにする。
    2. [Source] の設定で、Current Retarget Pose を「T Pose」に変更する。
    </details>
4. 右下の Asset Browser から好きなアニメーション（例：MF_Walk_Fwd）をダブルクリックするとプレビューできる。うまく動けばこれはOK。うまく動かなければ、いろいろ調べて直してね＾＾；
5. 流用したいアニメーションブループリント（今回の場合、ABP_Manny）の上で右クリックし、Retarget Animations を押す。
6. Target Skeletal Mesh にインポートしたモデルのスケルタルメッシュを設定。  
Auto Generate Retargeter のチェックを外し、
Retarget Asset に RTG_Mannequin をセットする。
7. エクスポートしたいアニメーション（今回は、ABP_Manny を選択）して、Export Animations
8. エクスポート先を設定して（たくさんのファイルが追加されるので新しいフォルダがおススメ）、必要ならばファイルの名前を変更するオプションを入力し、エクスポート、エクスポート。
9. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Animation > Anim Class にエクスポートしたアニメーションブループリントを設定。実行するとちゃんと動く。

### ジャンプ後の着地時に起こる異常の修正
- 上記の設定で実行してジャンプして着地すると、着地時に異常な表示が起こり、また、ゲーム終了時に「Trying to play a non-additive animation 'MM_Land_Aneki' into a pose that is expected to be additive in anim instance 'ABP_Aneki_C /Game/UEDPIE_0_Lobby.Lobby:PersistentLevel.BP_ThirdPersonCharacter_C_0.CharacterMesh0.ABP_Aneki_C_0’」のような警告が出る。
- これは、IK Retargeter のアニメーションエクスポート時に、アニメーションの Additive Settings が反映されないというバグらしい。ということで、この項目のみ手動で設定すればOK
1. 対象アニメーションの元ファイル(今回は、着地時のアニメーション MM_Land)を開く
2. エクスポート先アニメーションのファイル(今回は、MM_Land_Aneki)を開き、Additive Settings を 1. で開いたファイルと同じ状態にする。すると、治る。
