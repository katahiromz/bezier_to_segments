# bezier_to_segments

## 概要

JavaScriptでベジエ曲線を線分に分解します。

<img src="img/screenshot.png" alt="[スクリーンショット]" />

## コード

```js
/**
 * 3次ベジェ曲線を線分に分解した座標の配列を取得します。
 * @param {number} x0 始点のx座標
 * @param {number} y0 始点のy座標
 * @param {number} cp1x 制御点1のx座標
 * @param {number} cp1y 制御点1のy座標
 * @param {number} cp2x 制御点2のx座標
 * @param {number} cp2y 制御点2のy座標
 * @param {number} x3 終点のx座標
 * @param {number} y3 終点のy座標
 * @param {number} [segments=32] 分割数（線分の数）
 * @returns {Array<{x: number, y: number}>} 線分に分解された点の配列
 */
const getBezierSegments = (x0, y0, cp1x, cp1y, cp2x, cp2y, x3, y3, segments = 32) => {
    // 分割数が1未満の場合はエラーを防ぐために最低1に設定
    if (segments < 1) segments = 1;

    let points = [];

    // tは0から1まで変化するパラメータ
    for (let i = 0; i <= segments; i++) {
        const t = i / segments;

        // 3次ベジェ曲線のパラメトリック方程式
        // B(t) = (1-t)³P₀ + 3(1-t)²tP₁ + 3(1-t)t²P₂ + t³P₃

        const oneMinusT = 1 - t;
        const oneMinusTSquared = oneMinusT * oneMinusT;
        const oneMinusTCubed = oneMinusTSquared * oneMinusT;
        const tSquared = t * t;
        const tCubed = tSquared * t;

        // 各項の係数
        const factor0 = oneMinusTCubed;
        const factor1 = 3 * oneMinusTSquared * t;
        const factor2 = 3 * oneMinusT * tSquared;
        const factor3 = tCubed;

        // x座標の計算
        const x = factor0 * x0 + factor1 * cp1x + factor2 * cp2x + factor3 * x3;
        // y座標の計算
        const y = factor0 * y0 + factor1 * cp1y + factor2 * cp2y + factor3 * y3;

        points.push({ x: x, y: y });
    }

    return points;
}
```
