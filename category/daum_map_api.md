---
layout: category
title: Daum Map Api
---

다음 주소 api입니다.
우편번호 및 주소를 daumApi를 사용하여 편리하게 입력받을 수 있습니다.

HTML
```md
<form class="bq-event-content-group-form m0">
    <input type="text" class="bq-input col-sm-8 col-xs-12 p0 m0-0-1 radius1" id="contact4" name="event_addr" placeholder="배송지" value="<?=$event_addr?>">
</form>
<input type="button" onclick="daum_map_api_open()" value="우편번호 찾기"><br>
```

Javascript + Jquery
```md
function daum_map_api_open() {
    var width = 500;
    var height = 600;
    daum.postcode.load(function(){
        new daum.Postcode({
            oncomplete: function(data) {

                $('[name=event_addr]').val(data.zonecode + ' ' + data.roadAddress);
                console.log(data.zonecode + ' ' + data.roadAddress);
                console.log(data);
            }
        }).open({
            left: (window.screen.width / 2) - (width / 2),
            top: (window.screen.height / 2) - (height / 2)
        });
    });

}
```