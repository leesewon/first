---
layout: post
title: Javascript - cheackbox 
---

cheackbox - cheacked

간단하지만 헷갈리는 cheackbox 부분 정리입니다. 

HTML
```md
<input type="checkbox" name="policy_checkbox[]" class="policy_checkbox"/>
<input type="checkbox" name="policy_checkbox[]" class="policy_checkbox"/>
<input type="checkbox" name="policy_checkbox[]" class="policy_checkbox"/>

<input type="checkbox" id="policy_all"/>모두 동의합니다.</label>
```

JAVASCRIPT + JQUERY All Cheacked
```md
$(document).on('click','#policy_all', function(){
   $checked = $(this).is(":checked");
   $('.policy_checkbox').prop('checked', $checked);
});
```

JAVASCRIPT + JQUERY Cheacked 검사
```md
$is_checked = true;
$(".policy_checkbox").each(function() {
    $checked = $(this).is(":checked");
    if($checked == false) {
        $is_checked = false;
    }
});
if ($is_checked == false) {
    alert('이용약관을 동의해주세요.');
    return;
}
```
