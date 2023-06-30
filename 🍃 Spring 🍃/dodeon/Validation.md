# Validation, 유효성 체크

* 파일 형식이 올바른지
* 보내진 파일이 서버에 들어왔을 때 문제가 없는지

<br>
우리가 자바에서 Scanner 등을 사용해서 입력을 받는 경우 런타임 에러가 뜨는 경우가 있다.
우리는 이것을 잡아주기 위해서 Validation을 해주는데 서버도 마찬가지이다.
요청이 유효해야 서버가 요청을 보고 응답을 제대로 해줄 수 있기 때문에 이러첨 Validation을 해주는 것이다.

<br>

* 서버에서 하는 방법 : 자바 코드로 작성한다
* 클라이언트에서 하는 방법 : 자바스크립트로 작성한다
    * 하지만 이 경우 중간에 요청을 바꿀 수 있기 때문에 이것 하나만 사용하기에는 적절하지 않다.
* 위 두가지 방법을 적절한 비율로 사용해가면서 코드를 작성하는 것이 좋다

## 어떻게 하느냐

```java

@Slf4j
@Controller
@RequestMapping("/validation/v1/items")
@RequiredArgsConstructor
public class ValidationItemControllerV1 {

    @PostMapping("/add")
    public String addItem(@ModelAttribute Item item, RedirectAttributes redirectAttributes, Model model) {
        // 검증 오류를 결과를 보관하는 객체를 만든다.
        Map<String, String> errors = new HashMap<>();

        // 검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
            errors.put("itemName", "상품 이름은 필수입니다.");
        }

        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1_000_000) {
            errors.put("price", "가격은 1,000원에서 1,000,000원까지 허용합니다.");
        }

        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            errors.put("quantity", "수량은 최대 9,999까지 허용합니다.");
        }

        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();

            if (resultPrice < 10000) {
                // 여러 필드에 대한 오류는 필드 이름을 넣을 수 없으니 globalError로 넣는다.
                errors.put("globalError", "가격 * 수량은 10,000원 이상이어야 합니다.");
            }
        }

        // 에러가 있다면 입력 폼으로 다시 돌아간다.
        if (!errors.isEmpty()) {
            log.info("errors = {}", errors);
            model.addAttribute("errors", errors);
            return "validation/v1/addForm";
        }

        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);

        return "redirect:/validation/v1/items/{itemId}";
    }
}
```

이러첨 오류를 if문으로 모두 잡아주는 것이다.

## 문제점

* 뷰 템플릿에 중복 처리가 많다
* 타입 오류 처리가 안 된다
    * 숫자 타입에 문자가 들어가면 오류 발생
    * 이 오류는 400에러가 발생
    * 따라서 고객이 입력한 값이 별도로 관리되어야 한다

# BindingResult

* 스프링이 제공하는 검증 오류를 보관한다
* @ModelAttribute에 바인딩하다가 에더가 나도 컨트롤러가 호출된다
    * BindingResult가 없으면 400에러가 나면서 컨트롤러가 호출되지 않는다
    * 있으면 오류가 발생해도 오류 정보를 BindingResult에 담아 컨트롤러 호출
* 검증할 대상 바로 뒤에 와야 한다
* Model에 자동으로 포함되어 뷰로 넘어간다

```java

@Slf4j
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    @PostMapping("/add")
    // BindingResult는 꼭 @ModelAttribute 옆에 있어야 한다.
    public String addItemV1(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes) {
        // 검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
            bindingResult.addError(new FieldError("itemName", "itemName", "상품 이름은 필수입니다."));
        }

        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1_000_000) {
            bindingResult.addError(new FieldError("item", "price", "가격은 1,000원에서 1,000,000원까지 허용합니다."));
        }

        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", "수량은 최대 9,999까지 허용합니다."));
        }

        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();

            if (resultPrice < 10000) {
                bindingResult.addError(new ObjectError("item", "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
            }
        }

        if (bindingResult.hasErrors()) {
            log.info("errors={}", bindingResult);
            return "validation/v2/addForm";
        }

        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);

        return "redirect:/validation/v2/items/{itemId}";
    }
}
```

### BindingResult

@ModelAttribute 옆에 위치해야 어떤 데이터를 바인딩하는지 인식할 수 있다

### FieldError

* 필드 오류를 담는다
* ObjectError의 자식이다

```java
class FieldError extends ObjectError {
    public FieldError(String objectName, String field, String defaultMessage) {
    }
}
```

* ObjectError : @ModelAttribute 이름
* field : 오류가 발생한 필드 이름
* defaultMessage : 오류 기본 메시지

### ObjectError

글로벌 오류를 담는다

```java
class ObjectError {
    public ObjectError(String objectName, String defaultMessage) {
    }
}
```

* objectName : @ModelAttribute 이름
* defaultMessage : 오류 기본 메시지

# FieldError, ObjectError

사용자가 입력한 값이 오류 메시지가 발생해도 그대로 남아있도록 한다

* objectName : 오류가 발생한 객체 이름
* field : 오류가 발생한 필드
* rejectedValue : 사용자가 입력한 거절된 값
* bindingFailure : 타입 오류 등의 바인딩 실패인지, 검증 실패인지 구분하는 값
* codes : 메시지 코드
* arguments : 메시지에서 사용하는 인자
* defaultMessage: 기본 오류 메시지

```java

@Slf4j
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

    @PostMapping("/add")
    public String addItemV2(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes) {
        if (!StringUtils.hasText(item.getItemName())) {
            bindingResult.addError(new FieldError("itemName", "itemName", item.getItemName(), false, null, null, "상품 이름은 필수입니다."));
        }

        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1_000_000) {
            bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, null, null, "가격은 1,000원에서 1,000,000원까지 허용합니다."));
        }

        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, null, null, "수량은 최대 9,999까지 허용합니다."));
        }

        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();

            if (resultPrice < 10000) {
                // objectError는 기존 값이 있는 게 아니므로 fieldError 같은 생성자는 없다.
                bindingResult.addError(new ObjectError("item", null, null, "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
            }
        }

        if (bindingResult.hasErrors()) {
            log.info("errors={}", bindingResult);
            return "validation/v2/addForm";
        }

        // 성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);

        return "redirect:/validation/v2/items/{itemId}";
    }
}
```

이렇게 코드를 작성하면 에러가 발생하여 redirect를 해도 입력한 값이 그대로 폼에 남아있게 된다.

# 오류 코드와 메시지 처리

### FieldError 생성자

```properties
# messages와 errors 두 곳 모두 찾아 쓰도록 설정한다.
# 생략하면 기본값인 messages만 읽어들인다.
spring.messages.basename=messages,errors
```

```properties
required.item.itemName=상품 이름은 필수입니다.
range.item.price=가격은 {0} ~ {1} 까지 허용합니다.
max.item.quantity=수량은 최대 {0} 까지 허용합니다.
totalPriceMin=가격 * 수량의 합은 {0}원 이상이어야 합니다. 현재 값 = {1}
```

```java

@Slf4j
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {
    @PostMapping("/add")
    public String addItemV3(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes) {
        if (!StringUtils.hasText(item.getItemName())) {
            bindingResult.addError(new FieldError("itemName", "itemName", item.getItemName(), false, new String[]{"required.item.itemName"}, null, null));
        }

        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1_000_000) {
            bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, new String[]{"range.item.price"}, new Object[]{100, 1_000_000}, null));
        }

        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, new String[]{"max.item.quantity"}, new Object[]{9_999}, null));
        }

        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();

            if (resultPrice < 10000) {
                // objectError는 기존 값이 있는 게 아니므로 fieldError 같은 생성자는 없다.
                bindingResult.addError(new ObjectError("item", new String[]{"totalPriceMin"}, new Object[]{10_000, resultPrice}, null));
            }
        }

        ...

        return "redirect:/validation/v2/items/{itemId}";
    }
}
```

기존 application.properties에 설정을 추가한다. 컨트롤러에는 이제 defaultMessage 파라미터를 넘기는 대신에
code와 argument 파라미터에 데이터를 넘기면 errors.properties와 매핑된다.

* code : 메시지 코드를 지정
    * 배열이 들어가는 이유는 해당 키로 error.properties에서 값을 찾지 못했을 때 다음 값으로 찾을 수 있디 때문
* argument : properties의 {0} {1}을 치환한다

## rejectValue(), reject()

```java

@Slf4j
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {
    @PostMapping("/add")
    public String addItemV3(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes) {
    ...
    }
}
```

```
log.info("objectName={}",bindingResult.getObjectName());
        log.info("target={}",bindingResult.getTarget());
        
        
objectName=item // @ModelAttribute name
target=Item(id=null, itemName=상품, price=100, quantity=1234)
```
