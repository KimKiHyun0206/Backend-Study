# RequestBody
> HTTP 바디의 내용을 자바 객체로 변환해서 매핑된 메소드의 파라미터로 전달한다


    @RestController
    public class Controller{
    
        @RequestMapping("/sample")
        public String sample( @RequestBody UserVo userVo ) throws Exception {
            //변수 사용
        }
    }
위의 코드는 받은 요청을 변수로 받아와 사용하는 메소드이다

# ResponseBody
> 자바 객체를 HTTP요청의 바디 내용으로 매핑하여 클라이언트에 전송한다

    
    @ResponseBody
    @RequestMapping("/sample")
    public String sample() throws Exception{
        return ObjectValue;
    }
위의 코드는 HTTP에 `ObjectValue`라는 객체를 매핑하여 클라이언트에 전송한 것이다
