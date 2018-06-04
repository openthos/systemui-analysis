## 拼音输入流程：
- PinyinIME.java输入处理主类  
1.onKeyDown， onKeyUp -> processKey() 按键处理  
2.输入第一个拼音时mImeState == ImeState.STATE_IDLE -> processStateIdle修改mImeState为ImeState.STATE_INPUT；  
3.继续输入mImeState == ImeState.STATE_INPUT-> processStateInput处理拼音输入的主要方法；  
4.当输入为a~z的字母时，执行processSurfaceChange -> mDecInfo.addSplChar添加输入的拼音，chooseAndUpdate查询词库，选择候选词；  
5.chooseAndUpdate -> mDevInfo.chooseDecodingCandidate(int canId)当canId >= 0就选择一个候选词并刷新候选词列表，当canId < 0 对输入的拼音进6.行查询，查询调用mIPinyinDecoderService.imSearch();
- mIPinyinDecoderService.imSearch  
-> org_openthos_inputmethod_pinyin_PinyinDecoderService.cpp:nativaImSearch  
-> pinyinime.cpp:im_search  
-> matrixsearch.cpp:search, matrixsearch.cpp:get_candidate_num
