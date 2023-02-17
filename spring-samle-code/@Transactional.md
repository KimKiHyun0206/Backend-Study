# Transactional
> 데이터베이스에 접근하는 메소드에 붙여줌으로써 이 메소드가 데이터베이스에 영향을 끼친다는 것을 알림과 동시에 데이터베이스 트랜잭션을 사용하기 위한 어노테이션

## 데이터베이스 트랜잭션
* 원자성 : 모든 결과가 반영되거나 모든 결과가 반영되지 않아야 한다
* 지속성 : 트랜잭션이 성공적으로 완료될 경우, 완료된 결과를 영구적으로 반영해야 한다
* 독립성 : 지금 실행되는 트랜잭션에 다른 트랜잭션이 중간에 개입할 수 없다
* 일관성 : 트랜잭션 처리 결과는 항상 일관성을 가지고 있어야 한다


    위의 특징들을 지키기 위해서 @Transactional 어노테이션을 사용한다

## @Transactional

    public class SampleClass{
        
        @Transactional
        public void addUser(UserVO user){
            repository.save(user);
        }
    }
위의 코드는 유저를 삽입하는 코드이며 이 코드를 실행하다가 오류가 발생한다면. 모든 작업이 취소된다