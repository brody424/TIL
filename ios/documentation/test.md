
--- 원본

Because property wrapper syntax is just syntactic sugar for a property with a getter and a setter, accessing height and width behaves the same as accessing any other property.

For example, the code in resize(to:) accesses height and width using their property wrapper. 

If you call resize(to: .large), the switch case for .large sets the rectangle’s height and width to 100. 

The wrapper prevents the value of those properties from being larger than 12, and it sets the projected value to true, to record the fact that it adjusted their values. 

At the end of resize(to:), the return statement checks $height and $width to determine whether the property wrapper adjusted either height or width.


--- GOOGLE

속성 래퍼 Syntax는 getter 및 setter가있는 속성에 대한 구문 설탕이기 때문에 높이와 너비는 다른 속성에 액세스하는 것과 동일하게 작동합니다.

예를 들어, Resize의 코드는 속성 래퍼를 사용하여 높이와 너비에 액세스합니다.

resize (.large)를 호출하면 .large의 스위치 케이스는 사각형의 높이와 너비를 100으로 설정합니다.

래퍼는 해당 속성의 값이 12보다 크지 않도록 방지하고, 예상 값을 True로 설정하여 값을 조정했다는 사실을 기록합니다.

Resize (to :)의 끝에서, 반환 명령문은 속성 래퍼가 높이 또는 너비를 조정했는지 여부를 결정하기 위해 $ 높이와 $ 너비를 확인합니다.

--- PAPAGO

속성 래퍼 구문은 게터 및 세터가 있는 속성에 대한 구문 설탕이므로 높이 및 너비에 액세스하는 것은 다른 속성에 액세스하는 것과 동일하게 동작합니다.

예를 들어, 크기 조정(to:)의 코드는 속성 래퍼를 사용하여 높이와 너비에 액세스합니다. 

크기 조정(to: .large)을 호출하면 .large의 스위치 케이스는 직사각형의 높이와 너비를 100으로 설정합니다. 

포장지는 해당 속성의 값이 12보다 크지 않도록 방지하고 예측 값을 true로 설정하여 값을 조정했다는 사실을 기록합니다. 

크기 조정(to:)이 끝나면 반환문은 $high와 $width를 확인하여 속성 포장지가 높이 또는 너비 중 하나를 조정했는지 확인합니다.

--- GPT

속성 래퍼 구문은 속성 게터와 세터가 있는 속성의 구문 설탕일 뿐이므로, 높이(height)와 너비(width)에 액세스하는 것은 다른 속성에 액세스하는 것과 동일합니다.

예를 들어, resize(to:)의 코드는 높이와 너비를 그들의 속성 래퍼를 사용하여 액세스합니다.

만약 resize(to: .large)를 호출하면, .large에 대한 switch case는 사각형의 높이와 너비를 100으로 설정합니다.

래퍼는 이러한 속성들의 값이 12보다 크지 않도록 방지하고, 그들의 값이 조정되었다는 사실을 기록하기 위해 프로젝션 값을 true로 설정합니다.

resize(to:)의 끝에서, return 문은 $height와 $width를 확인하여 속성 래퍼가 높이나 너비 중 하나를 조정했는지를 결정합니다.