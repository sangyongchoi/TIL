# Creating-multi-modules(with-IntelliJ)

1. IntelliJ 를 활용하여 새로운 프로젝트를 생성합니다.
    
    ![Untitled](Creating-multi-modules(with-IntelliJ)%20111bac45157c4276be4682192b1eb876/Untitled.png)
    

1. build.gradle.kts 의 내용을 아래와 같이 변경합니다.
plugins 는 apply false 를 적용해줍니다.
    
    ![Untitled](Creating-multi-modules(with-IntelliJ)%20111bac45157c4276be4682192b1eb876/Untitled%201.png)
    

1. 프로젝트 우클릭 → New → Module 을 선택하여 새로운 모듈을 만들어줍니다.
    
    ![Untitled](Creating-multi-modules(with-IntelliJ)%20111bac45157c4276be4682192b1eb876/Untitled%202.png)
    
2. 새로만들어진 모듈의 build.gradle.kts 를 아래와 같이 변경합니다.
plugins 의 version 을 없애면 됩니다.
    
    ![Untitled](Creating-multi-modules(with-IntelliJ)%20111bac45157c4276be4682192b1eb876/Untitled%203.png)
    

1. root 의 setting.gradle.kts 에서 새로 만들 모듈을 include 해줍니다.
    
    ![Untitled](Creating-multi-modules(with-IntelliJ)%20111bac45157c4276be4682192b1eb876/Untitled%204.png)