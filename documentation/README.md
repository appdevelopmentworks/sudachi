# 🍃 スダチ 3Dモデル - Three.js用エクスポート

## 📁 ファイル構成

### 🎯 Three.js用モデルファイル

**推奨形式:**
- `sudachi_collection.glb` - 全スダチモデル（単一ファイル）
- `sudachi_collection.gltf` + `.bin` - 全スダチモデル（分離ファイル）

**代替形式:**
- `sudachi_collection.obj` + `.mtl` - Wavefront OBJ形式

### 📂 individual_sudachi/ フォルダ
個別のスダチモデル（GLB形式）:
- `sudachi_1.glb` - スダチ1（約14万ポリゴン）
- `sudachi_2.glb` - スダチ2（約14万ポリゴン）
- `sudachi_3.glb` - スダチ3（約14万ポリゴン）
- `sudachi_4.glb` - スダチ4（約13万ポリゴン）
- `sudachi_5.glb` - スダチ5（約13万ポリゴン）
- `cut_sudachi.glb` - 半分カットスダチ（約8万ポリゴン）

### 🌐 ビューワー
- `three_js_sudachi_viewer.html` - 3Dビューワー（ブラウザで直接表示）

---

## 🚀 使用方法

### 1. 簡単なビューワー使用
```bash
# HTMLファイルをブラウザで開く
start three_js_sudachi_viewer.html
```

### 2. Three.jsプロジェクトに組み込み

#### GLB形式の読み込み例:
```javascript
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

const loader = new GLTFLoader();

// 全体のスダチコレクション
loader.load('path/to/sudachi_collection.glb', (gltf) => {
    const sudachiScene = gltf.scene;
    scene.add(sudachiScene);
});

// 個別のスダチ
loader.load('path/to/individual_sudachi/sudachi_1.glb', (gltf) => {
    const sudachi = gltf.scene;
    scene.add(sudachi);
});
```

#### 基本的な設定:
```javascript
// シーン設定
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();

// ライティング（スダチの深緑色が映える設定）
const ambientLight = new THREE.AmbientLight(0xffffff, 0.4);
scene.add(ambientLight);

const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
directionalLight.position.set(10, 10, 5);
scene.add(directionalLight);
```

---

## 🎨 モデル仕様

### 色彩情報
- **基本色**: 極深緑 RGB(0.05, 0.16, 0.03)
- **影色**: 最深緑 RGB(0.03, 0.12, 0.02)
- **ハイライト**: 中深緑 RGB(0.08, 0.20, 0.05)
- **断面果肉**: クリーム色 RGB(0.98, 0.96, 0.90)

### 技術仕様
- **形式**: glTF 2.0 / GLB / OBJ
- **座標系**: Y-up（Three.js標準）
- **ポリゴン数**: 8,000～140,000（モデルによる）
- **テクスチャ**: プロシージャルマテリアル
- **法線**: 含まれる
- **UV**: 含まれる

### マテリアル特性
- **Roughness**: 0.8（マットな質感）
- **Metallic**: 0.0（金属感なし）
- **ノイズテクスチャ**: 3層構造（150/45/6スケール）
- **バンプマッピング**: 柑橘類の皮の凹凸

---

## 🎯 最適化のヒント

### パフォーマンス
- **LOD使用**: 距離に応じて個別モデルと全体モデルを切り替え
- **Frustum Culling**: カメラ外のオブジェクトを非表示
- **レベル選択**: 用途に応じてポリゴン数を選択

### 表示品質
- **アンチエイリアシング**: WebGLRendererで有効化
- **シャドウ**: PCFSoftShadowMapを推奨
- **トーンマッピング**: ACESFilmicToneMappingを推奨

---

## 📋 対応ブラウザ

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

---

## 🍃 スダチについて

徳島県特産の香酸柑橘類スダチを忠実に3D再現:
- **実物参考**: 写真データからの色彩調整
- **リアル質感**: プロシージャルテクスチャ
- **高精細**: Subdivision Surface適用
- **写実的**: 実際のスダチの深緑色を再現

---

## 📝 制作情報

- **制作ツール**: Blender 4.4 + Python automation
- **エクスポート**: glTF-Blender-IO addon
- **最適化**: Draco圧縮対応
- **制作日**: 2025年1月

---

## 🚨 注意事項

1. **ファイルサイズ**: 高精細モデルのため数MB～数十MB
2. **GPU要求**: 高ポリゴンモデルのため高性能GPU推奨
3. **メモリ使用量**: 全体表示時は大容量メモリが必要
4. **読み込み時間**: 初回読み込みに数秒かかる場合があります

---

## 🔧 カスタマイズ例

### 色の調整
```javascript
// マテリアルの色を調整
model.traverse((child) => {
    if (child.isMesh) {
        child.material.color.setRGB(0.05, 0.16, 0.03); // より深い緑
    }
});
```

### アニメーション追加
```javascript
// 回転アニメーション
function animate() {
    requestAnimationFrame(animate);
    
    if (sudachiModel) {
        sudachiModel.rotation.y += 0.01;
    }
    
    renderer.render(scene, camera);
}
```

### インタラクション
```javascript
// クリックで色変更
function onMouseClick(event) {
    const mouse = new THREE.Vector2();
    mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
    mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
    
    const raycaster = new THREE.Raycaster();
    raycaster.setFromCamera(mouse, camera);
    
    const intersects = raycaster.intersectObjects(scene.children, true);
    if (intersects.length > 0) {
        const object = intersects[0].object;
        object.material.color.setHex(Math.random() * 0xffffff);
    }
}
```

---

## 🎮 ビューワー操作

### マウス操作
- **左ドラッグ**: モデル回転
- **ホイール**: ズーム
- **右クリック**: コンテキストメニュー

### キーボード操作
- **R**: カメラリセット
- **W**: ワイヤーフレーム切り替え
- **A**: 自動回転ON/OFF
- **1-6**: 個別モデル表示

---

## 📊 パフォーマンス情報

### ファイルサイズ
- `sudachi_collection.glb`: 約15-20MB
- `individual_sudachi/*.glb`: 約2-4MB（各）
- `sudachi_collection.obj`: 約25-30MB

### 推奨システム要件
- **CPU**: Intel i5 / AMD Ryzen 5以上
- **GPU**: GeForce GTX 1060 / Radeon RX 580以上
- **RAM**: 8GB以上
- **ブラウザ**: WebGL 2.0対応

---

## 🛠️ トラブルシューティング

### よくある問題

**Q: モデルが表示されない**
A: 
1. ブラウザがWebGLに対応しているか確認
2. ファイルパスが正しいか確認
3. コンソールでエラーメッセージを確認

**Q: 動作が重い**
A:
1. 個別モデル（sudachi_*.glb）を使用
2. レンダラーの解像度を下げる
3. 影やアンチエイリアシングを無効化

**Q: 色が正しく表示されない**
A:
1. ライティングを調整
2. トーンマッピングを設定
3. ガンマコレクションを有効化

---

## 🔗 参考リンク

- [Three.js公式ドキュメント](https://threejs.org/docs/)
- [glTF仕様](https://github.com/KhronosGroup/glTF)
- [WebGL仕様](https://www.khronos.org/webgl/)

---

## 📄 ライセンス

このスダチ3Dモデルは教育・研究目的で自由に使用できます。
商用利用の場合は制作者にご相談ください。

---

**🍃 美しいスダチの3Dモデルをお楽しみください！**