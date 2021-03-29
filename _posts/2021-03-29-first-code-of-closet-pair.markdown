import java.util.ArrayList;
import java.util.Comparator;
import java.util.StringTokenizer;
import java.awt.Point;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException; //throw를 이용하기 위해 import
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;


public class Dot {
    public static void main (String[] args) throws NumberFormatException, IOException
    {
        //BufferedReader 선언, InputStream으로 데이터를 문자 단위로 읽게 변형
        // BufferedWriter 선언, OutputStream으로 문자 단위에서 바이트 단위로 쓰게 변형
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());  // Read룰 String으로 했으므로 형변환 불필요
        ArrayList<Point> arrList = new ArrayList<>(); //Point 객체들만 add 될 수 있음
        for (int i = 0; i < N; i++) {

            //StringTokenizer을 이용해서 문자열을 분리함
            StringTokenizer st = new StringTokenizer(br.readLine());  //입력 문자열 넣음

            //Read한 데이터는 Line단위로만 나눠지기 때문에 공백단위로 구분하기 위해서는 StringTokenizer에 nextToken함수를 씀
            int x = Integer.parseInt(st.nextToken());  //int
            int y = Integer.parseInt(st.nextToken());  //int

            arrList.add(new Point(x, y)); //좌표상에서의 x값, y값 추가
        }

        arrList.sort(Comparator.comparingInt(p -> p.x));  // x좌표를 오름차순으로 정렬.


        //BufferedWriter는 flush와 close를 호출해서 처리해야 함
        bw.write(cp (arrList, 0, N - 1) + "\n");  //콘솔 창에 버퍼에 있는 값을 출력
        bw.flush();   //남아있는 데이터 모두 출력
        bw.close();   //스트림 닫음
        br.close();   //스트림 닫음

    }


    public static int l(Point p, Point q)  //l = 두 p,q 사이의 거리의 제곱
    {
        return (p.x - q.x) * (p.x - q.x) + (p.y - q.y) * (p.y - q.y);
    }


    //bruteForce(완전탐색) 이용해서 제일 가까운 거리 찾기
    static int bruteForce(ArrayList<Point> arrList, int start, int end)
    {
        int c = Integer.MAX_VALUE;  //c는 정수의 최대값
        for (int i = start; i < end; i++) {
            for (int j = i + 1; j <= end; j++) {
                int k = l(arrList.get(i), arrList.get(j));
                c = Math.min(k, c);
            }
        }

        return c;
    }


    public static int cp (ArrayList<Point> arrList, int start, int end) {
        int n = end - start + 1;


        if (n <= 3) // n이 3 이하일때 bruteForce로 가까운 두 점의 거리 찾음
        {
            return bruteForce(arrList, start, end);
        }

        int m = (start + end) / 2;

        int d = Math.min(cp(arrList, start, m), cp(arrList, m + 1, end));


        //기준에서 d 만큼의 영역을 area라고 함
        //d 이내의 area 구하기
        ArrayList<Point> area = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            int t = arrList.get(m).x - arrList.get(i).x;

            if (t * t < d) {
                area.add(arrList.get(i));
            }
        }

        area.sort(Comparator.comparingInt(p -> p.y));  //y좌표를 오름차순으로 정렬

        //반복문을 이용해서 d이내의 y좌표인 점들을 비교
        for (int i = 0; i < area.size() - 1; i++) {
            for (int j = i + 1; j < area.size(); j++) {
                int t = area.get(j).y - area.get(i).y;

                if (t * t < d)
                {
                    d = Math.min(d, l(area.get(i), area.get(j)));
                } else  //중복되지 않게 하기 위해 d보다 거리가 멀어지면 반복문을 종료
                    {
                    break;
                }
            }
        }

        return d;
    }

}