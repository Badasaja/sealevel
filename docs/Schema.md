## 통합 경로 탐색 스키마 명세 (도보/차량 반영)

차량과 도보 네트워크를 단일 DB에서 관리하되, 쿼리 시 `allowed_mode`로 필터링하여 연산 부하를 통제하는 구조임.

### 1. 노드 테이블 (Nodes)

- **Table:** `road_nodes`
    
- **DDL 구조:**
    
    - `node_id` (BigInt, PK): 노드 고유 ID.
        
    - `lon` (Double): 경도.
        
    - `lat` (Double): 위도.
        
    - `elevation_m` (Float): DEM 추출 해발고도(m).
        
    - `geom` (Geometry(Point, 4326)): 공간 인덱스용.
        

### 2. 엣지 테이블 (Edges / Links)

- **Table:** `road_edges`
    
- **DDL 구조:**
    
    - `edge_id` (BigInt, PK): 엣지 고유 ID.
        
    - `source_node` (BigInt, FK): 시작 노드.
        
    - `target_node` (BigInt, FK): 종료 노드.
        
    - `length_m` (Float): 물리적 길이(m).
        
    - **`allowed_mode` (VarChar(10)):** 'drive', 'walk', 'both'. (라우팅 핵심 필터키).
        
    - **`is_oneway` (Boolean):** 차량 기준 일방통행 여부. (도보 쿼리 시 무시됨).
        
    - **`infra_type` (VarChar(20)):** 'surface', 'bridge', 'tunnel', 'stairs'. (침수 면역 및 보행 페널티 판별용).
        
    - **`incline_penalty` (Float):** 고도차 기반 도보 가중치 (경사도).
        
    - `geom` (Geometry(LineString, 4326)): 선형 공간 데이터.
        

### 3. 침수 매핑 테이블 (Flood Status)

- **Table:** `edge_flood_status`
    
- **DDL 구조:**
    
    - `edge_id` (BigInt, FK): 대상 엣지.
        
    - `scenario_cm` (Integer): 상승 시나리오 (예: 50, 100).
        
    - `is_flooded` (Boolean): 침수 여부 판별값.
        

---

