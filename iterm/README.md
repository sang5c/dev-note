### iTerm2에서 opt+방향키, cmd+방향키 사용하기

1. 키 바인딩
    ```bash
    bindkey "[D" backward-word
    bindkey "[C" forward-word
    bindkey "^[a" beginning-of-line
    bindkey "^[e" end-of-line
    ```
2. 단축키 해제
   - [cmt + ,] -> Keys -> Key Bindings 진입
   - 단축키 [cmd + <-] 기능을 [Action: SendEscape Sequence, Esc+: a]로 변경
   - 단축키 [cmd + ->] 기능을 [Action: SendEscape Sequence, Esc+: e]로 변경
