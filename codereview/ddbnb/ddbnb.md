---
layout: post
title: 코드 리뷰 - 펫 맘 모집 웹 애플리케이션
tags: [React, Redux, JavaScript]
permalink: ddbnb/code-review/
---

**펫 맘 모집 웹 애플리케이션 코드 리뷰**

### 프론트엔드 코드 리뷰

#### `getPetMomList` 함수

- 코드 설명: 펫맘 리스트 조회를 위한 함수입니다.
- 주요 기능: 현재 페이지와 검색 조건을 기반으로 펫맘 리스트를 조회합니다.
- 리뷰 내용:
  - 함수가 현재 페이지와 검색 조건을 받아 API를 호출하고, 결과를 Redux store에 저장하는 역할을 수행합니다.
  - URL을 동적으로 생성하여 검색 조건이 존재하는 경우 해당 조건을 쿼리 문자열로 추가합니다.
  - 비동기 함수로 API 요청을 보내고, 응답을 JSON 형식으로 파싱하여 결과를 저장합니다.
  - 응답이 성공적인 경우 Redux 액션을 통해 결과를 저장하고, 이를 컴포넌트에서 활용합니다.

#### `putPetMomPage` 함수

- 코드 설명: 이 함수는 [설명을 추가하세요].
- 주요 기능: [주요 기능을 설명하세요].
- 리뷰 내용: [리뷰 내용을 추가하세요].

### `PetMomList` 함수

- 코드 설명:
- 주요 기능:
- 리뷰 내용:

```javascript
import "../../css/petmomList.css";
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { useNavigate } from "react-router-dom";
import { getPetMomList } from "../../api/petMomAPI";
import PageBtn from "../../component/common/PageBtn";
import PetMomOptionBtn from "../../component/item/PetMomOptionBtn";
import SearchBar from "../../component/item/SearchBar";
import PetMomCard from "../../component/list/PetMomCard";

function PetMomList() {
  const dispatch = useDispatch();
  const navigate = useNavigate();

  const token = JSON.parse(window.localStorage.getItem("accessToken"));
  const postUser = token !== null ? true : false;

  const { data: petmomList, pageInfo } = useSelector(
    (state) => state.petMomReducer
  );

  const searchValue = useSelector((state) => state.searchReducer);
  const currentPage = useSelector((state) => state.pageReducer);

  useEffect(() => {
    dispatch(getPetMomList(currentPage, searchValue));
  }, [currentPage, searchValue]);

  return (
    <div>
      {postUser && (
        <button className="write" onClick={() => navigate("./recruit")}>
          글쓰기
        </button>
      )}
      <br />
      <div className="main">
        <SearchBar Option={PetMomOptionBtn} />
      </div>
      <br />
      {Array.isArray(petmomList) &&
        petmomList.map((petmom) => (
          <PetMomCard petmom={petmom} key={petmom.boardId} />
        ))}
      <PageBtn pageInfo={pageInfo} />
    </div>
  );
}

export default PetMomList;
```

#### getPetMomList 함수

- 코드 설명: 이 함수는 펫맘 리스트를 조회하는 데 사용됩니다. 현재 페이지와 검색 조건에 따라 API를 호출하고, 결과를 Redux 스토어에 저장합니다.

- 주요 기능:
  - 현재 페이지와 검색 조건을 기반으로 동적 URL 생성
  - API 요청을 비동기적으로 보내고 응답을 JSON으로 파싱
  - 응답이 성공적인 경우 Redux 액션을 통해 데이터 저장

```javascript
export const getPetMomList = (currentPage, searchValue) => {
  let URL = `http://${process.env.REACT_APP_RESTAPI_URL}/api/v1/petMom?page=${currentPage}`;

  // 검색 조건이 존재하는 경우 해당 조건을 쿼리 문자열로 추가
  if (searchValue?.location !== "") {
    URL += `&location=${searchValue?.location}`;
  }

  if (searchValue?.startDate !== "") {
    URL += `&startDate=${searchValue?.startDate}`;
  }

  if (searchValue?.endDate !== "") {
    URL += `&endDate=${searchValue?.endDate}`;
  }

  if (searchValue?.petYN !== "") {
    URL += `&petYN=${searchValue?.petYN}`;
  }

  if (searchValue?.otherCondition !== "") {
    URL += `&otherCondition=${searchValue?.otherCondition}`;
  }

  return async (dispatch, getState) => {
    try {
      const result = await fetch(URL, {
        method: "GET",
        headers: {
          "Content-type": "application/json",
          Accept: "*/*",
        },
      }).then((response) => response.json());

      if (result.status === 200) {
        dispatch({ type: GET_PETMOM, payload: result.data });
      }
    } catch (error) {
      console.error("Error fetching pet mom list:", error);
    }
  };
};
```

### 결론

이 코드 리뷰를 통해 펫 맘 모집 웹 애플리케이션의 프론트엔드와 백엔드 부분을 검토하였습니다. 프로젝트 개요와 주요 코드 기능에 대한 설명을 포함하여 코드의 가독성을 높이고, 역할과 기여 내용을 간결하게 나타냈습니다. 이 코드 리뷰를 통해 프로젝트의 코드 품질과 안전성을 확인하고 지속적인 개선을 진행하고 있음을 강조합니다.
