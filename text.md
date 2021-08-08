한국어 번역

소개
배운 것을 활용하여 유령의 집을 만들어 봅시다. 우리는 Three.js 프리미티브를 지오메트리, /static/textures/폴더 의 텍스처 , 하나 또는 두 개의 새로운 기능 으로만 사용할 것 입니다.
지붕, 문 및 일부 덤불로 구성된 초등학교 집을 만들 것입니다. 우리는 또한 정원에 무덤을 만들 것입니다. 시트로 만든 눈에 보
우리는 벽, 이는 유령 대신에 우리는 단순히 벽과 바닥을 통과하며 떠다니는 여러 가지 빛깔의 조명을 사용할 것입니다.

측정 팁
프리미티브를 사용하여 무언가를 만들 때 우리가 항상 범하는 초보자 실수 중 하나는 임의 측정을 사용하는 것입니다. Three.js의 하나의 단위는 원하는 모든 것을 의미할 수 있습니다.

하늘을 날 수 있는 상당한 풍경을 만들고 있다고 가정해 보겠습니다. 이 경우 1단위를 1킬로미터로 생각할 수 있습니다. 집을 짓는다면 1단위를 1미터로 생각할 수 있고, 대리석 게임을 만든다면 1단위를 1센티미터로 생각할 수 있습니다.

특정 단위 비율이 있으면 기하 도형을 만드는 데 도움이 됩니다. 당신이 문을 만들고 싶다고 가정해 봅시다. 문이 자신보다 약간 높으므로 2미터 정도에 도달해야 합니다.

영국식 단위를 사용하는 경우 변환을 수행해야 합니다.

설정
스타터는 바닥, 구체, 일부 조명(유령의 집에는 너무 강렬함)으로만 구성되어 있으며 그림자도 작동하지 않습니다.

우리는 집을 직접 만들고 더 나은 분위기를 위해 현재 조명을 조정하고 그림자를 추가하고 나머지를 추가해야 합니다.

/assets/lessons/16/step-01.png

집
먼저 구를 제거하고 작은 집을 만들어 보겠습니다. 우리는 바닥을 떠날 수 있습니다.

/assets/lessons/16/step-02.png

그 집을 구성하는 모든 객체를 장면에 넣는 대신 전체를 이동하거나 크기를 조정하려는 경우를 대비하여 먼저 컨테이너를 만듭니다.

// House container
const house = new THREE.Group()
scene.add(house)
자바스크립트
그런 다음 간단한 큐브로 벽을 만들고 에 추가할 수 있습니다 house. y축 에서 벽을 위로 이동하는 것을 잊지 마십시오 . 그렇지 않으면 바닥 내부의 절반이 됩니다.

const walls = new THREE.Mesh(
new THREE.BoxGeometry(4, 2.5, 4),
new THREE.MeshStandardMaterial({ color: '#ac8e82' })
)
walls.position.y = 1.25
house.add(walls)
자바스크립트
/assets/lessons/16/step-03.png

우리는 2.5천장의 일반적인 높이처럼 보이기 때문에 높이를 선택했습니다 . 우리는 또한 '#ac8e82'색상을 선택 했지만 일시적이며 나중에 해당 색상을 텍스처로 교체합니다.

지붕의 경우 피라미드 모양을 만들고 싶습니다. 문제는 Three.js에 이런 종류의 지오메트리가 없다는 것입니다. 그러나 원뿔에서 시작하여 면의 수를 로 줄이면 4피라미드가 됩니다. 때로는 기본 기능 사용을 편향시켜야 합니다.

// Roof
const roof = new THREE.Mesh(
new THREE.ConeGeometry(3.5, 1, 4),
new THREE.MeshStandardMaterial({ color: '#b35f45' })
)
roof.rotation.y = Math.PI \* 0.25
roof.position.y = 2.5 + 0.5
house.add(roof)
자바스크립트
/assets/lessons/16/step-04.png

올바른 위치와 올바른 회전을 찾는 것이 약간 어려울 수 있습니다. 시간을 갖고 가치 뒤에 있는 논리를 알아내려고 노력하고 그것이 Math.PI당신의 친구 라는 것을 잊지 마십시오 .

보시다시피 우리는 떠났습니다 2.5 + 0.5. 작성할 수도 3있지만 값 뒤에 있는 논리를 시각화하는 것이 더 좋습니다. 2.5, 지붕 벽이 2.5단위 높이이고 0.5원뿔이 1단위 높이 이기 때문입니다 (높이의 절반까지 이동해야 함).

이전 단원에서 사용한 아름다운 문 텍스처를 사용할 것이므로 문에 간단한 평면을 사용합니다.

// Door
const door = new THREE.Mesh(
new THREE.PlaneGeometry(2, 2),
new THREE.MeshStandardMaterial({ color: '#aa7b7b' })
)
door.position.y = 1
door.position.z = 2 + 0.01
house.add(door)
자바스크립트
/assets/lessons/16/step-05.png

평면의 크기가 올바른지 아직 알 수 없지만 나중에 텍스처가 작동하면 수정할 수 있습니다.

보시다시피 z축의 문을 이동 하여 벽에 고정하지만 0.01단위 도 추가했습니다 . 이 작은 값을 추가하지 않으면 z-fighting이라는 이전 단원에서 이미 본 버그가 있습니다. Z-파이팅은 두 얼굴이 같은 위치(또는 매우 가까운 위치)에 있을 때 발생합니다. GPU는 어느 것이 다른 것보다 더 가까운지 알지 못하며 이상한 시각적 픽셀 싸움이 발생합니다.

덤불을 몇 개 추가해 보겠습니다. 각 수풀에 대해 하나의 지오메트리를 생성하는 대신 하나만 생성하고 모든 메시가 이를 공유합니다. 결과는 시각적으로 동일하지만 성능이 향상됩니다. 우리는 재료로 똑같이 할 수 있습니다.

// Bushes
const bushGeometry = new THREE.SphereGeometry(1, 16, 16)
const bushMaterial = new THREE.MeshStandardMaterial({ color: '#89c854' })

const bush1 = new THREE.Mesh(bushGeometry, bushMaterial)
bush1.scale.set(0.5, 0.5, 0.5)
bush1.position.set(0.8, 0.2, 2.2)

const bush2 = new THREE.Mesh(bushGeometry, bushMaterial)
bush2.scale.set(0.25, 0.25, 0.25)
bush2.position.set(1.4, 0.1, 2.1)

const bush3 = new THREE.Mesh(bushGeometry, bushMaterial)
bush3.scale.set(0.4, 0.4, 0.4)
bush3.position.set(- 0.8, 0.1, 2.2)

const bush4 = new THREE.Mesh(bushGeometry, bushMaterial)
bush4.scale.set(0.15, 0.15, 0.15)
bush4.position.set(- 1, 0.05, 2.6)

house.add(bush1, bush2, bush3, bush4)
자바스크립트
/assets/lessons/16/step-06.png

이러한 모든 개체를 코드에 직접 배치하고 크기를 조정하려면 너무 오래 걸립니다. 나중 강의에서 3D 소프트웨어를 사용하여 이 모든 것을 만드는 방법을 배웁니다.

우리는 앞으로 나아가야 하기 때문에 집에 너무 많은 세부 사항을 추가하지 않을 것이지만 낮은 벽, 골목, 창문, 굴뚝, 바위 등과 같이 원하는 것을 자유롭게 일시 중지하고 추가하십시오.

무덤
각 무덤을 수동으로 배치하는 대신 절차에 따라 생성하고 배치합니다.

아이디어는 무덤을 집 주변의 원에 무작위로 배치하는 것입니다.

먼저 다음과 같은 경우에 대비하여 컨테이너를 생성해 보겠습니다.

// Graves
const graves = new THREE.Group()
scene.add(graves)
자바스크립트
에서와 마찬가지로 3D 텍스트 우리는 하나 개의 형상과 하나의 재료로 여러 도넛을 만들어 수업, 우리는 하나 만들려고하고있다 BoxGeometry 한 MeshStandardMaterial 모든 무덤 사이에 공유됩니다 :

const graveGeometry = new THREE.BoxGeometry(0.6, 0.8, 0.2)
const graveMaterial = new THREE.MeshStandardMaterial({ color: '#b2b6b1' })
자바스크립트
마지막으로 집 주변에 무덤을 배치하기 위해 몇 가지 수학을 반복하고 수행해 보겠습니다.

우리는 원에 임의의 각도를 만들 것입니다. 완전한 회전은 2의 π임을 기억하십시오. 그런 다음 이 각도를 sin(...)와 에 모두 사용합니다 cos(...). 이것은 각도가 있을 때 원에 물건을 배치하는 방법입니다. 그리고 마지막으로 우리는 무덤이 완벽한 원에 위치하는 것을 원하지 않기 때문에 이들 sin(...)과 cos(...)결과에 임의의 값을 곱합니다 .

for(let i = 0; i < 50; i++)
{
const angle = Math.random() _ Math.PI _ 2 // Random angle
const radius = 3 + Math.random() _ 6 // Random radius
const x = Math.cos(angle) _ radius // Get the x position using cosinus
const z = Math.sin(angle) \* radius // Get the z position using sinus

    // Create the mesh
    const grave = new THREE.Mesh(graveGeometry, graveMaterial)

    // Position
    grave.position.set(x, 0.3, z)

    // Rotation
    grave.rotation.z = (Math.random() - 0.5) * 0.4
    grave.rotation.y = (Math.random() - 0.5) * 0.4

    // Add to the graves container
    graves.add(grave)

}
자바스크립트
/assets/lessons/16/step-07.png

등
우리는 꽤 멋진 장면을 가지고 있지만 아직 무섭지 않습니다.

먼저 앰비언트 라이트와 달 라이트를 어둡게 하고 더 푸른빛이 도는 색상을 지정해 보겠습니다.

const ambientLight = new THREE.AmbientLight('#b9d5ff', 0.12)

// ...

const moonLight = new THREE.DirectionalLight('#b9d5ff', 0.12)
자바스크립트
/assets/lessons/16/step-08.png

지금은 많이 볼 수 없습니다. 문 위에 따뜻한 PointLight 도 추가해 보겠습니다 . 이 조명을 장면에 추가하는 대신 집에 추가할 수 있습니다.

// Door light
const doorLight = new THREE.PointLight('#ff7d46', 1, 7)
doorLight.position.set(0, 2.2, 2.7)
house.add(doorLight)
자바스크립트
/assets/lessons/16/step-09.png

안개
공포 영화에서는 항상 안개를 사용합니다. 좋은 소식은 Three.js가 이미 Fog 클래스 에서 지원한다는 것 입니다.

첫 번째 매개변수는 color이고 두 번째 매개변수는 near(카메라에서 안개가 시작되는 거리), 세 번째 매개변수는 far(카메라에서 안개가 완전히 불투명해지는 거리)입니다.

안개를 활성화하려면 fog속성을 다음에 추가 하십시오 scene.

/\*\*

- Fog
  \*/
  const fog = new THREE.Fog('#262837', 1, 15)
  scene.fog = fog
  자바스크립트
  /assets/lessons/16/step-10.png

나쁘지는 않지만 무덤과 검은 배경 사이의 깨끗한 절단을 볼 수 있습니다.

이를 수정하려면 의 맑은 색상을 변경 renderer하고 안개와 동일한 색상을 사용해야 합니다. 인스턴스화한 후 다음을 수행하십시오 renderer.

renderer.setClearColor('#262837')
자바스크립트
/assets/lessons/16/step-11.png

조금 더 무서운 장면이 있습니다.

텍스처
더 사실적인 느낌을 주기 위해 텍스처를 추가할 수 있습니다. 는 textureLoader코드 이미 사용 중입니다.

문
우리가 이미 알고 있는 것으로 시작하여 모든 문 텍스처를 로드해 보겠습니다.

const doorColorTexture = textureLoader.load('/textures/door/color.jpg')
const doorAlphaTexture = textureLoader.load('/textures/door/alpha.jpg')
const doorAmbientOcclusionTexture = textureLoader.load('/textures/door/ambientOcclusion.jpg')
const doorHeightTexture = textureLoader.load('/textures/door/height.jpg')
const doorNormalTexture = textureLoader.load('/textures/door/normal.jpg')
const doorMetalnessTexture = textureLoader.load('/textures/door/metalness.jpg')
const doorRoughnessTexture = textureLoader.load('/textures/door/roughness.jpg')
자바스크립트
그런 다음 모든 텍스처를 문 재질에 적용할 수 있습니다. PlaneGeometry 에 더 많은 세분화를 추가하는 것을 잊지 마십시오 . 따라서 displacementMap이동할 정점이 몇 개 있습니다. 또한 재료 수업 에서 했던 것처럼 에 uv2대한 지오메트리에 속성을 추가합니다 .aoMap

다음을 사용하여 문의 형상에 액세스할 수 있습니다 mesh.geometry.

const door = new THREE.Mesh(
new THREE.PlaneGeometry(2, 2, 100, 100),
new THREE.MeshStandardMaterial({
map: doorColorTexture,
transparent: true,
alphaMap: doorAlphaTexture,
aoMap: doorAmbientOcclusionTexture,
displacementMap: doorHeightTexture,
displacementScale: 0.1,
normalMap: doorNormalTexture,
metalnessMap: doorMetalnessTexture,
roughnessMap: doorRoughnessTexture
})
)
door.geometry.setAttribute('uv2', new THREE.Float32BufferAttribute(door.geometry.attributes.uv.array, 2))
자바스크립트
/assets/lessons/16/step-12.png

저기요! 더 현실적인 문입니다.

이제 텍스처가 생겼으므로 문이 너무 작다는 것을 알 수 있습니다. PlaneGeometry 크기를 간단히 늘릴 수 있습니다 .

// ...
new THREE.PlaneGeometry(2.2, 2.2, 100, 100),
// ...
자바스크립트
/assets/lessons/16/step-13.png

벽
/static/textures/bricks/폴더 의 텍스처를 사용하여 벽에 대해서도 동일한 작업을 수행해 보겠습니다 . 문만큼 텍스처가 많지는 않지만 문제가 되지는 않습니다. 우리는 알파 텍스처가 필요하지 않고 벽에 금속이 없으므로 금속성 텍스처도 필요하지 않습니다.

텍스처 로드:

const bricksColorTexture = textureLoader.load('/textures/bricks/color.jpg')
const bricksAmbientOcclusionTexture = textureLoader.load('/textures/bricks/ambientOcclusion.jpg')
const bricksNormalTexture = textureLoader.load('/textures/bricks/normal.jpg')
const bricksRoughnessTexture = textureLoader.load('/textures/bricks/roughness.jpg')
자바스크립트
그런 다음 벽에 대한 MeshStandardMaterial 을 업데이트할 수 있습니다 . 주변 폐색에 대한 속성 을 제거 color하고 추가하는 것을 잊지 마십시오 uv2.

const walls = new THREE.Mesh(
new THREE.BoxGeometry(4, 2.5, 4),
new THREE.MeshStandardMaterial({
map: bricksColorTexture,
aoMap: bricksAmbientOcclusionTexture,
normalMap: bricksNormalTexture,
roughnessMap: bricksRoughnessTexture
})
)
walls.geometry.setAttribute('uv2', new THREE.Float32BufferAttribute(walls.geometry.attributes.uv.array, 2))
자바스크립트
/assets/lessons/16/step-14.png

바닥
벽과 같은 거래. 잔디 텍스처는 /static/textures/grass/폴더에 있습니다.

텍스처 로드:

const grassColorTexture = textureLoader.load('/textures/grass/color.jpg')
const grassAmbientOcclusionTexture = textureLoader.load('/textures/grass/ambientOcclusion.jpg')
const grassNormalTexture = textureLoader.load('/textures/grass/normal.jpg')
const grassRoughnessTexture = textureLoader.load('/textures/grass/roughness.jpg')
자바스크립트
바닥 의 MeshStandardMaterial 을 업데이트하고 주변 폐색에 대한 속성 을 제거 color하고 추가하는 것을 잊지 마십시오 uv2.

const floor = new THREE.Mesh(
new THREE.PlaneGeometry(20, 20),
new THREE.MeshStandardMaterial({
map: grassColorTexture,
aoMap: grassAmbientOcclusionTexture,
normalMap: grassNormalTexture,
roughnessMap: grassRoughnessTexture
})
)
floor.geometry.setAttribute('uv2', new THREE.Float32BufferAttribute(floor.geometry.attributes.uv.array, 2))
자바스크립트
/assets/lessons/16/step-15.png

텍스처가 너무 큽니다. 이 문제를 해결하려면 repeat속성을 사용하여 각 잔디 텍스처를 반복하면 됩니다.

grassColorTexture.repeat.set(8, 8)
grassAmbientOcclusionTexture.repeat.set(8, 8)
grassNormalTexture.repeat.set(8, 8)
grassRoughnessTexture.repeat.set(8, 8)
자바스크립트
/assets/lessons/16/step-16.png

반복을 활성화하려면 wrapS및 wrapT속성을 변경하는 것을 잊지 마십시오 .

grassColorTexture.wrapS = THREE.RepeatWrapping
grassAmbientOcclusionTexture.wrapS = THREE.RepeatWrapping
grassNormalTexture.wrapS = THREE.RepeatWrapping
grassRoughnessTexture.wrapS = THREE.RepeatWrapping

grassColorTexture.wrapT = THREE.RepeatWrapping
grassAmbientOcclusionTexture.wrapT = THREE.RepeatWrapping
grassNormalTexture.wrapT = THREE.RepeatWrapping
grassRoughnessTexture.wrapT = THREE.RepeatWrapping
자바스크립트
/assets/lessons/16/step-17.png

유령
유령을 위해, 일을 단순하게 유지하고 우리가 아는 대로 합시다.

우리는 집 주위를 떠다니며 땅과 무덤을 통과하는 간단한 조명을 사용할 것입니다.

/\*\*

- Ghosts
  \*/
  const ghost1 = new THREE.PointLight('#ff00ff', 2, 3)
  scene.add(ghost1)

const ghost2 = new THREE.PointLight('#00ffff', 2, 3)
scene.add(ghost2)

const ghost3 = new THREE.PointLight('#ffff00', 2, 3)
scene.add(ghost3)
자바스크립트
이제 우리는 삼각법이 많이 포함된 일부 수학을 사용하여 애니메이션을 만들 수 있습니다.

const clock = new THREE.Clock()

const tick = () =>
{
const elapsedTime = clock.getElapsedTime()

    // Ghosts
    const ghost1Angle = elapsedTime * 0.5
    ghost1.position.x = Math.cos(ghost1Angle) * 4
    ghost1.position.z = Math.sin(ghost1Angle) * 4
    ghost1.position.y = Math.sin(elapsedTime * 3)

    const ghost2Angle = - elapsedTime * 0.32
    ghost2.position.x = Math.cos(ghost2Angle) * 5
    ghost2.position.z = Math.sin(ghost2Angle) * 5
    ghost2.position.y = Math.sin(elapsedTime * 4) + Math.sin(elapsedTime * 2.5)

    const ghost3Angle = - elapsedTime * 0.18
    ghost3.position.x = Math.cos(ghost3Angle) * (7 + Math.sin(elapsedTime * 0.32))
    ghost3.position.z = Math.sin(ghost3Angle) * (7 + Math.sin(elapsedTime * 0.5))
    ghost3.position.y = Math.sin(elapsedTime * 4) + Math.sin(elapsedTime * 2.5)

    // ...

}
자바스크립트
그림자
마지막으로 사실감을 더하기 위해 그림자를 추가해 보겠습니다.

렌더러에서 그림자 맵을 활성화합니다.

renderer.shadowMap.enabled = true
자바스크립트
그림자를 드리워야 한다고 생각하는 조명의 그림자를 활성화합니다.

moonLight.castShadow = true
doorLight.castShadow = true
ghost1.castShadow = true
ghost2.castShadow = true
ghost3.castShadow = true
자바스크립트
장면의 각 개체를 살펴보고 해당 개체가 그림자를 드리우거나 받을 수 있는지 결정합니다.

walls.castShadow = true
bush1.castShadow = true
bush2.castShadow = true
bush3.castShadow = true
bush4.castShadow = true

for(let i = 0; i < 50; i++)
{
// ...
grave.castShadow = true
// ...
}

floor.receiveShadow = true
자바스크립트
/assets/lessons/16/step-19.png

이 그림자로 장면이 훨씬 좋아 보이지만 어쨌든 최적화해야 합니다.

좋은 일이, 각각의 빛을 통해 이동에 카메라 헬퍼를 작성하는 것 light.shadowMap.camera, 그리고 확인하기 near는 far의 amplitude또는 fov맞게 잘. 대신 다음과 같이 맞아야 하는 값을 사용합시다.

성능을 향상시키기 위해 섀도우 맵 렌더 크기를 줄일 수도 있습니다.

moonLight.shadow.mapSize.width = 256
moonLight.shadow.mapSize.height = 256
moonLight.shadow.camera.far = 15

// ...

doorLight.shadow.mapSize.width = 256
doorLight.shadow.mapSize.height = 256
doorLight.shadow.camera.far = 7

// ...

ghost1.shadow.mapSize.width = 256
ghost1.shadow.mapSize.height = 256
ghost1.shadow.camera.far = 7

// ...

ghost2.shadow.mapSize.width = 256
ghost2.shadow.mapSize.height = 256
ghost2.shadow.camera.far = 7

// ...

ghost3.shadow.mapSize.width = 256
ghost3.shadow.mapSize.height = 256
ghost3.shadow.camera.far = 7

// ...

renderer.shadowMap.type = THREE.PCFSoftShadowMap
자바스크립트
/assets/lessons/16/step-20.png

이 과정은 길지만 필수적입니다. 우리는 이미 성능 제한에 시달렸고, 유령의 집은 모바일에서 60fps에서도 작동하지 않을 수 있습니다. 다음 강의에서 더 많은 최적화 팁을 볼 것입니다.

더 나아가
이것이 수업을 위한 것입니다. 하지만 우리가 한 일을 개선하기 위해 노력할 수 있습니다. 장면에 새 요소를 추가하고, Three.js 프리미티브를 사용하여 유령을 실제 3D 유령으로 교체하고, 무덤에 이름을 추가하는 등의 작업을 수행할 수 있습니다.
