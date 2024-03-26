### Lab 4-3을 활용하여 three.js로 3D 큐브를 회전 하는 코드를 작성하시오.

### 실습 화면 (3초 마다 랜덤한 색상으로 변경되는 회전하는 정육면체)

![Animation](https://github.com/qkrgudals1030/aigrapics/assets/50895124/aa7e6ebc-e3df-4043-876f-5bf4df790074)

챗지피티를 활용하여 js 코드부분을 수정해 파란색 정육면체가 아닌 3초마다 랜덤한 색상으로 변경되면서 회전하는 정육면체로 수정하였습니다.

### 소감 

전찬빈 : 이론에 비례 듣다보니 어떤 쓰임새인지 이해는 되었다. 큐브를 직접 조정하고 색조정 속도조정 등 다양한 기능을 통해 접근을 하여 이제 시작이다 라는 느낌이 들었다. 다만 안보고 할 자신은 없어 반복학습이 필요할 것 같았다. 그리고 이론으로 들은 설명을 잘 정리하지 않으면 정말 힘들것 같다.

김성현 : 이전의 간단한 편집기에 이어 드디어 정말로 본격적인 그래픽기술 배우기에 들어갔다. 사이트를 통해 집을 만들어볼 때 부터 어릴적 게임을 처음 할 때의 재미와 '앞으로 무엇을 하게될까' 라는 기대감을 오랜 시간이 지나 다시한번 느낄 수 있었던 순간이었다. 물론 아직은 시작단계이니 간단하게 큐브를 만들어 돌려보는 것으로 끝났지만, 이전의 집과는 다르게 본격적으로 뭔가를 만들어냈다는 느낌이 들었다. 기본 요소를 알아가기 위해 만드는 것인 만큼 빛, 도형의 재질, 카메라 등 그래픽 작업에 들어가는 필수요소들을 한 눈에 보고 알아갈 수 있었다. 이전 시간에도 그랬듯이 '수업을 듣는다' 가 아닌 '뭔가 재미있는 것을 한다' 라는 느낌을 받았기에 어쩌면 이 쪽으로 공부하는 것이 좀 더 좋을지도 모르겠다는 생각도 들었다.

김태윤 : 실습을 하기에 앞서 어떤 내용이 어떤작용을 하는지 이해는 되어있어 쉽게 진행이 될줄 알았으나 생각보다 오류도 많이 나오고, 원하는 결과가 나오지 않아 고생했지만 팀원들의 도움과 교수님의 강의자료를 활용하여 성공하였을때 저는 감동해버렸습니다.추가로 새로운 기능을 넣을때 여러 시도를 해보며 공부가 된거같습니다. 코드를 보면서 하여도 오류가 많았던 점을 감안하면 앞으로 공부를 더 열심히 해야할거 같습니다.

박형민 : three.js를 활용하여 다양한 과제를 수행하는 과정에서 저번 과제인 집을 제작해보는 과제는 단순히 오브젝트를 조합하여 집을 만들어 보았다면 오늘 실습해본 회전하는 정육면체같은 경우에는 코딩을 통해 제작해보면서 매일매일 새로운 경험을 해볼 수 있었습니다. 이런 다양한 경험을 차곡차곡 쌓아 저만의 것으로 만들어 저만의 작품을 만들 수 있도록 열심히 공부해 보는 계기가 될 수 있었습니다. ai그래픽스 화이팅!!


### .css 코드
```
* {
    outline: none;
    margin: 0;
}
body {
    overflow: hidden;
}
#webgl-container{
    position: absolute;
    top: 0;
    left: 0;
    width: 70%;
    height: 100%;
}

```

### .js 코드
```
import * as THREE from '../build/three.module.js';

class App {
    constructor() {
        const divContainer = document.querySelector("#webgl-container");
        this._divContainer = divContainer;

        const renderer = new THREE.WebGL1Renderer({antialias: true});
        renderer.setPixelRatio(window.devicePixelRatio);
        divContainer.appendChild(renderer.domElement);
        this._renderer = renderer;

        const scene = new THREE.Scene();
        this._scene = scene;

        this._setupCamera();
        this._setupLight();
        this._setupModel();

        window.onresize = this.resize.bind(this);
        this.resize();

        requestAnimationFrame(this.render.bind(this));

        // 색상 변경 타이머 설정
        this.colorChangeInterval = 3; // 3초마다 색상 변경
        this.lastColorChangeTime = 0;
    }

    _setupCamera() {
        const width = this._divContainer.clientWidth;
        const height = this._divContainer.clientHeight;
        const camera = new THREE.PerspectiveCamera(
            75,
            width / height,
            0.1,
            100
        );
        camera.position.z = 2;
        this._camera = camera;
    }

    _setupLight() {
        const color = 0xffffff;
        const intensity = 1;
        const light = new THREE.DirectionalLight(color, intensity);
        light.position.set(-1, 2, 4);
        this._scene.add(light);
    }

    _setupModel() {
        const geometry = new THREE.BoxGeometry(1, 1, 1);
        const material = new THREE.MeshPhongMaterial({color: 0x44a88});

        const cube = new THREE.Mesh(geometry, material);
        this._scene.add(cube);
        this._cube = cube;
    }

    resize() {
        const width = this._divContainer.clientWidth;
        const height = this._divContainer.clientHeight;

        this._camera.aspect = width / height;
        this._camera.updateProjectionMatrix();

        this._renderer.setSize(width, height);
    }

    render(time) {
        this._renderer.render(this._scene, this._camera);
        this.update(time);
        requestAnimationFrame(this.render.bind(this));
    }

    update(time) {
        time *= 0.001;
        this._cube.rotation.x = time;
        this._cube.rotation.y = time;

        // 3초마다 색상 변경
        if (time - this.lastColorChangeTime > this.colorChangeInterval) {
            const randomColor = Math.random() * 0xffffff;
            this._cube.material.color.set(randomColor);
            this.lastColorChangeTime = time;
        }
    }
}

window.onload = function() {
    new App()
}

```
### .html 코드
```
<!DOCTYPE html>
<html>
    <head>
        <meta value="viewport" content="width=device-width, inital-scale=1">
        <link rel="stylesheet" href="01_basic.css">
        <script type="importmap">
			{
				"imports": {
					"three": "../build/three.module.js",
					"three/addons/": "./jsm/"
				}
			}
		</script>
        <script type="module" src="01_basic.js" defer></script>
    </head>
    <body>
        <div id="webgl-container"></div>
    </body>
</html>

```
