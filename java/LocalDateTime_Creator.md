# LocalDateTime 인스턴스가 생성되는 과정

LocalDateTime는 (구)자바에서 쓰였던 날짜/시간 관련 클래스인 Date, Calendar의 단점을 극복하고자 자바 8에서 추가된 java.time 패키지의 하위 클래스다. Date와 Calendar 클래스는 가변 클래스였지만 LocalDateTime은 불변 클래스로 생성되어.. 불변 클래스를 되집어 보고자.. 뜯어보았다.

정적 팩터리 메서드인 of 함수를 이용해 인스턴스를 만들 수 있다.

<br>

LocalDateTime.of
```java
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second) {
    LocalDate date = LocalDate.of(year, month, dayOfMonth);
    LocalTime time = LocalTime.of(hour, minute, second);
    return new LocalDateTime(date, time);
}
```
LocalDateTime은 날짜와 시간, 두 가지 개념을 가지고 있는 클래스로 <br>
날짜만을 표현하는 LocalDate 클래스와 시간만을 표현하는 LocalTime 클래스를 final 변수로 가진다.

<br>

LocalDate.of
```java
public static LocalDate of(int year, Month month, int dayOfMonth) {
    YEAR.checkValidValue(year);
    Objects.requireNonNull(month, "month");
    DAY_OF_MONTH.checkValidValue(dayOfMonth);
    return create(year, month.getValue(), dayOfMonth);
}
```
LocalDate도 LocalDateTime과 마찬가지로 of 메서드를 이용해 인스턴스를 생성한다.
LocalDate에서는 날짜 정보를 직접 관리하기 때문에 년/월/일 값에 대한 유효성 체크를 진행한 후, 팩터리 메서드 create를 호출한다.


LocalDate.create
```java
private static LocalDate create(int year, int month, int dayOfMonth) {
    if (dayOfMonth > 28) {
        int dom = 31;
        switch (month) {
            case 2:
                dom = (IsoChronology.INSTANCE.isLeapYear(year) ? 29 : 28);
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                dom = 30;
                break;
        }
        if (dayOfMonth > dom) {
            if (dayOfMonth == 29) {
                throw new DateTimeException("Invalid date 'February 29' as '" + year + "' is not a leap year");
            } else {
                throw new DateTimeException("Invalid date '" + Month.of(month).name() + " " + dayOfMonth + "'");
            }
        }
    }
    return new LocalDate(year, month, dayOfMonth);
}
```
앞서 of 메서드에서는 년/월/일 각각의 값에 대한 유효성 검사를 했다면, create 메서드에서는 각 정보를 조합한 유효성 검사를 진행한다. 윤년에 대한 검사 또한 이 과정에서 진행한다.

<br>

LocalDate 생성자
```java
/**
* Constructor, previously validated.
**/
private LocalDate(int year, int month, int dayOfMonth) {
    this.year = year;
    this.month = (short) month;
    this.day = (short) dayOfMonth;
}
```
private 생성자를 두어 외부에서 함부로 생성할 수 없도록 제한을 두었다. <br>
더불어 주석으로 각 필드값의 유효성 체크가 필요하다는 것을 고지한다.

<br>

```java
private LocalDateTime(LocalDate date, LocalTime time) {
    this.date = date;
    this.time = time;
}
```
최종적으로 LocalDateTime 또한 private로 생성자를 이용해 인스턴스를 생성한다.
