# Guardian Tales - DirectX 11 C++ 학습 프로젝트

> C++과 DirectX 11을 사용한 가디언 테일즈 모작 프로젝트입니다.  
> 캐릭터 전투와 퍼즐 상호작용을 구현하고, 맵 제작을 위한 편집 도구를 구성했습니다.

---

## 영상

<a href="https://youtu.be/o8DtHcs6mCU">
 <img width="1672" height="941" alt="2022-07-06 13-36-10 mkv_20261015_053926 800" src="https://github.com/user-attachments/assets/2567ceaa-a8c6-4cbc-856e-129e5dd2dd0f" />
</a>

- [전체 시연 영상](https://youtu.be/o8DtHcs6mCU)

---

## 1. 프로젝트 개요

| 항목 | 내용 |
|---|---|
| 프로젝트명 | 가디언 테일즈 모작 |
| 장르 | 2D 액션 RPG / 퍼즐 |
| 사용 기술 | C++ / DirectX 11 / HLSL / ImGui |
| 개발 기간 | 8주 |
| 개발 인원 | 2명 |
| 담당 범위 | ImGui 기반 맵·이벤트 편집 도구, 전체 캐릭터의 공격·스킬, 동료 AI, OBB 충돌 판정, 일부 보스의 공격 패턴 구현 |

---

## 2. 핵심 구현

### 2.1 ImGui·셰이더 기반 맵 툴과 에디터

ImGui에서 타일 배치, 맵 크기 설정, 충돌 영역 편집과 저장·불러오기를 할 수 있도록 했습니다. 타일별 UV 정보를 StructuredBuffer로 셰이더에 전달해, 픽셀 위치에 맞는 타일 이미지를 표시했습니다.

에디터의 목록 선택과 드래그 앤 드롭은 객체와 멤버 함수 포인터를 등록하는 델리게이트 방식으로 연결했습니다.

<img width="1154" height="352" alt="image" src="https://github.com/user-attachments/assets/14e41bb6-f58b-4883-9504-cc99cfbbdcd9" />

### 2.2 퍼즐 오브젝트의 조건 연결

여러 오브젝트의 활성 상태를 연결해, 모든 조건이 충족되면 문 등의 장치가 작동하도록 했습니다.

한 번 활성화되면 유지하는 방식과 조건이 풀리면 다시 비활성화되는 방식을 구분했습니다. 에디터에서 연결을 추가·해제하고, 연결 관계를 파일로 저장·복원할 수 있도록 했습니다.

<img width="426" height="240" alt="Video Project 71" src="https://github.com/user-attachments/assets/46a039bc-7897-400b-869e-b1d6aae1ba4b" />

### 2.3 동료의 자동 추적·전투

적이 없을 때는 파티의 앞 순서 캐릭터를 따라가고, 거리에 따라 정지·걷기·달리기를 전환했습니다.

적을 감지하면 대상에게 접근하고, 캐릭터의 공격 사거리 안에 들어오면 자동으로 공격과 콤보를 이어가도록 했습니다.

| 동료의 자동 추적 | 동료의 자동 전투 |
|:---:|:---:|
| <img width="426" height="240" alt="Video Project 72" src="https://github.com/user-attachments/assets/eed6d8fe-e604-4385-9d04-30a130b4fefd" /> | <img width="426" height="240" alt="Video Project 73" src="https://github.com/user-attachments/assets/babf6698-9b6f-4a69-a5c5-b84082a250e8" /> |

### 2.4 OBB 충돌 판정

분리축 정리(SAT)를 이용해 회전된 사각형 충돌체의 겹침 여부를 판정했습니다. 두 사각형의 가로·세로 방향을 기준으로 투영해, 분리되는 축이 있으면 충돌하지 않은 것으로 처리했습니다.

충돌체 쌍의 이전 상태를 저장해 충돌 진입·유지·이탈을 구분하고, 각 오브젝트의 스크립트에 전달했습니다.

<img width="674" height="459" alt="image" src="https://github.com/user-attachments/assets/41a26ae4-4e98-4413-b9eb-e10a8cd2b5e8" />
