* 설명

내용은 이해를 했는데 전체코드를짜는데 어려움이 있어서 파이썬 코드를 보고 자바로 재해석하면서 풀어봤습니다.

아직 전체적인 틀을 짜기에는 알고리즘 실력이 많이 부족한 것 같지만, 코드 재해석하면서 열심히 공부해봤습니다 ㅠㅠ!

Comparator 나 Array 함수를 이용하는건 자바의정석 2권을 참고하면서 짜봤습니다.

인터페이스와 내부클래스까지 공부는 어느정도 되어있어서 객체 배열을 사용하는건 크게 어렵지않았는데
고급함수기능들을 사용하지 않으면서 전체 코드를 짜려고하니 매우 힘들었습니다.

입력값과 결과값출력은 백준에서 주어진 입력값으로 확인했습니다.
문제없이 잘 출력되었습니다.



	
	import java.util.*;

	class Point{
    	 int x;
    	 int y;
 	}

	public class PointFind {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
            	int number = scan.nextInt();
            	Point[] p = new Point[number];
            	for (int i = 0; i < number; i++) {
            		p[i].x = scan.nextInt();
            		p[i].y = scan.nextInt();
            	     }

     		Arrays.sort(p, new Comparator<Point>() {                  
            		@Override
            		public int compare(Point o1, Point o2) {	//x좌표들 오름차순으로 정리.
                				return 0;
            			}
        			    });
        			System.out.println(Find(p,0, number-1));
    		 }

    	public static int distance(Point p1,Point p2){
        		return (int) (Math.pow((p1.x - p2.x),2) + Math.pow((p1.y - p2.y),2));
    	}

	// 분할정복을 이용해서 최소거리를 찾아준다. 이때 메인함수의 array sort때문에 x를 기준으로 오름차순 정렬이 되어있는 상태이다.
    	public static int Find(Point[] p,int left, int right) {
        		int list_len = right-left;
        		if (list_len == 1) {
            		return distance(p[left], p[left + 1]);
        		}
        		else if (list_len == 2) {
            		return Math.min(Math.min(distance(p[left], p[left + 1]), distance(p[left], p[left + 2])), distance(p[left + 1], p[left + 2]));
        		}
        		else {
            		int mid = (left + right) / 2;
            		int min_d = Math.min(Find(p,left, mid), Find(p,mid, right));

            		int midx = p[mid].x;

            		//최소거리를 기준으로 y축으로 나눈 중간 기준부터 x좌표의 거리들이 최소거리 안에 드는 점들을  새로운 리스트에 보관해준다.
            		ArrayList<Point> point2 = new ArrayList<>();
            		for (int i = left; i < right; i++) {
                			if (Math.pow((p[i].x - midx), 2) <= min_d)
                    				point2.add(p[i]);
            			}

            //새로만든 리스트를 y좌표들 오름차순으로 정리해준다.
            int seo = point2.toArray().length;
            if (seo >= 2) {
                point2.sort(new Comparator<Point>() {        
                    @Override
                    public int compare(Point o1, Point o2) {
                        return 0;
                    }
                });
                //정렬된 점들중 y좌표끼리 거리가 최소값을 넘으면 종료하고 아니면 그점들의 진짜 최솟값을 구해서 다시 최소거리를 구해준다.
                for (int i = 0; i < seo - 1; i++) {
                    for (int j = i + 1; j < seo; j++) {
                        if (Math.pow((point2.get(i).y - point2.get(j).y), 2) > min_d)
                            break;
                        else
                            min_d = Math.min(min_d, distance(point2.get(i), point2.get(j)));
                    }
                }
                
            }
            return min_d;
        }
    }
}
