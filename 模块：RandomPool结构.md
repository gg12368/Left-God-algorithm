设计RandomPool结构
【题目】 设计一种结构，在该结构中有如下三个功能：
insert(key)：将某个key加入到该结构，做到不重复加入。
delete(key)：将原本在结构中的某个key移除。
getRandom()： 等概率随机返回结构中的任何一个key。
【要求】 Insert、delete和getRandom方法的时间复杂度都是 O(1)

要求等概率解决思路：建立两个哈希表，只有add和random的时候，第一个表中用于存放数据，同时记录size大小，第二个表中存放size和对应的key，对于random返回的时候，则再size中随机抽出一个，然后在第二个表中返回key。
public static class Pool<k>{
	private HashMap<K,Integer> keyIndexMap;
	private HashMap<Integer,K> indexKeyMap;
	private int size;

	public Pool(){
		this.keyIndexMap = new HashMap<K,Integer>();
		this.indexKeyMap = new HashMap<Integer,K>();
		this.size = 0;
	}
	//将不重复的key放到hash表中非常简单，只需要简单判断目前不存在key，然后放入key和size就可以
	public void insert(K key){
		if(!this.keyIndexMap.containsKey(key)){
			this.keyIndexMap.put(key,this.size);
			this.indexKeyMap.put(this.size++,key);
		}
	}
	//删除key，将需要删除的key和最后一个key进行调换，然后删除最后一条数据，保证整个区间不会出现洞。
	public void delete(K key){
		//先判断是否包含所要删除的key
		if(this.keyIndexMap.containsKey(key)){
			//将key对应的size值读取出来，赋值给deleteIndex
			int deleteIndex = this.keyIndexMap.get(key);
			//将最后的size读取出来
			int lastIndex = --this.size;
			//将最后一个key读取出来
			K lastKey = this.indexKeyMap.get(lastIndex);
			this.keyIndexMap.put(lastKey,deleteIndex);
			this.indexKeyMap.put(deleteIndex,lastKey);
			this.keyIndexMap.remove(key);
			this.indexKeyMap.remove(lastIndex);
		}
	}
	//利用size长度进行等概率获取任意key
	public K getRandom(){
		if(this.size== 0){
			return null;
		}
		int randomIndex = (int) (Math.random()*this.size);
		return this.indexKeyMap.get(randomIndex);
	}
}
