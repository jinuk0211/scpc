응, 공부해서 풀 수 있는 문제야.
다만 이건 “LLM 잘 쓰는 문제”라기보다 채점 규칙/생성 규칙을 역추적하는 문제에 가까워.
지금 네 위치는 나쁘지 않아. 0.8435면 기본 구조는 이미 많이 맞춘 거고, 남은 건 대충 이런 것들임:
어떤 record가 최신 신호인지
route_binding_order가 focal 선택에 어떻게 먹히는지
dispatch_authority_check와 share_boundary_update 중 뭐가 우선인지
ambiguous_target일 때 ask인지 amend인지
local_update_boundary가 진짜 local proceed인지, redacted dispatch인지
plan_events.args가 어떤 ontology 값으로 정규화되는지
이건 공부 방향만 맞으면 풀 수 있음. 반대로 자연어 prompt 읽으면서 “상식적으로 이럴 듯” 하면 계속 틀림.
공부 순서는 이렇게 가면 돼.
채점 함수 완전히 이해
focal, target, control이 틀리면 뒤 점수가 거의 죽음. 그래서 plan보다 먼저 이 3개를 맞춰야 함.

dev 정답 120개를 외우듯 분석
각 task마다 records → 정답이 왜 그렇게 됐는지 표로 만들어야 함.

screening 700개를 record 조합별로 클러스터링
prompt보다 records 조합이 중요함. 같은 template이면 정답 규칙도 비슷할 가능성이 큼.

v5가 내는 답을 template별로 보기
한 template 안에서 ask/proceed/amend/hold가 이상하게 섞이면 그 함수가 아직 우선순위를 잘못 잡은 거임.

넓은 규칙 금지
v8처럼 “local_authority_confirmed면 proceed” 식으로 넓게 밀면 망함. 한 template 근거가 있을 때만 고쳐야 함.

그래서 답은: 네가 공부해서 풀 수 있다.
근데 “코드 조금 만져서 운 좋게 0.9”가 아니라, dev 정답으로 출제자의 finite-state rule을 복원하는 작업이라고 보면 돼.
시간이 별로 없으면 전부 공부하지 말고 딱 하나만 봐:
screening_tasks.jsonl에서 v5가 가장 불안한 route_binding_order / share_boundary_update / authority / route_candidate_snapshot 조합. 여기서 hidden 손실이 제일 클 가능성이 높아.
