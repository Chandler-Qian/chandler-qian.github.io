---
layout: post
title:  "Design Log Storage System"
date:   2020-02-23 14:36:23
permalink: /first-post.html
categories: problems
---
<span class="image featured"><img src="/images/file_system.jpg" alt=""></span>
This is a [leetcode problem](https://leetcode.com/problems/design-log-storage-system/).

A better displayed version can be found [here](https://github.com/Chandler-Qian/chandler-qian.github.io/blob/master/_posts/2020-02-23-Design_log_storage_system.markdown).

## Problem Discription

You are given several logs that each log contains a unique id and timestamp. Timestamp is a string that has the following format: <code>Year:Month:Day:Hour:Minute:Second</code>, for example, <code>2017:01:01:23:59:59</code>. All domains are zero-padded decimal numbers.

Design a log storage system to implement the following functions:

 - <code>void Put(int id, string timestamp)</code>: Given a log's unique
   id and timestamp, store the log in your storage system.
   
 - <code>int[] Retrieve(String start, String end, String
   granularity)</code>: Return the id of logs whose timestamps are
   within the range from start to end. Start and end all have the same
   format as timestamp. However, granularity means the time level for consideration. For example, start = <code>"2017:01:01:23:59:59"</code>, end = <code>"2017:01:02:23:59:59"</code>, granularity = "Day", it means that we need to find the logs within the range from Jan. 1st 2017 to Jan. 2nd 2017.

## My Solution
View my posted solution [here](https://leetcode.com/problems/design-log-storage-system/discuss/499375/best-solution-java-using-treemap-comparator-string-array-as-key).

* In order to store the timestamp -> log id mapping, we can use a string array as a key. TreeMap is useful for it makes keys sorted. Thus we need to construct our own comparator. 
* The key of retrieve function is that we need to construct a lower bound and upper bound for comparison. For lower bound, we can simply replace tail part with "00". For upper bound, we can replace the tail part with corresponding upper bound.

Example: 
<code>2016:08:21:03:12:09 </code>

 - Lowerbound (by **YEAR**): `2016:00:00:00:00:00`
 - Upperbound: `2016:12:31:23:59:59`

```
class LogSystem {
    
    private TreeMap<String[], Integer> map;
    private Map<String, Integer> timeMap;
    private Map<Integer, String> endBoundMap;
    private Comparator<String[]> comparator;
    
    public LogSystem() {
        comparator = new Comparator<String[]>(){
            @Override
            public int compare(String[] s1, String[] s2) {
                for (int i = 0; i < s1.length; i++) {
                    if (!s1[i].equals(s2[i])) {
                        return s1[i].compareTo(s2[i]);
                    }
                }
                return 0;
            }
        };
        
        map = new TreeMap<>(comparator);
        // load gra - index mapping
        timeMap = new HashMap<>();
        timeMap.put("Year", 0);
        timeMap.put("Month", 1);
        timeMap.put("Day", 2);
        timeMap.put("Hour", 3);
        timeMap.put("Minute", 4);
        timeMap.put("Second", 5);
        
        endBoundMap = new HashMap<>();      
        // load index - time end bound mapping
        endBoundMap.put(1, "12");
        endBoundMap.put(2, "31");
        endBoundMap.put(3, "23");
        endBoundMap.put(4, "59");
        endBoundMap.put(5, "59");
    }
    
    public void put(int id, String timestamp) {
        map.put(timestamp.split(":"), id);
    }
    
    public List<Integer> retrieve(String s, String e, String gra) {
        String[] start = s.split(":");
        String[] end = e.split(":");
        for (int i = timeMap.get(gra) + 1; i < start.length; i++) {
            start[i] = "00";
            end[i] = endBoundMap.get(i);
        }
        List<Integer> res = new ArrayList<>();
        for (String[] key: map.keySet()) {
            if (comparator.compare(start, key) <= 0) {
                if (comparator.compare(key, end) <= 0) res.add(map.get(key));
                else break;
            } 
        }
        return res;
    }
}

/**
 * Your LogSystem object will be instantiated and called as such:
 * LogSystem obj = new LogSystem();
 * obj.put(id,timestamp);
 * List<Integer> param_2 = obj.retrieve(s,e,gra);
 */
 ```