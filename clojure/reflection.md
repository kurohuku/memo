# ClojureやJavaの情報を扱う

-------------------------------------------------
## クラスの関係を取得する

Clojureでのクラスなどの関係は、以下の2つによって表現されていると思われます。

 - Javaの継承(extends)、実装(implements)の関係
 - derive関数で決められた hierarchy 中の親子関係
 

Clojureでクラスなどの関係を調べる方法としては、

 - Javaのメソッド(.getSuperclassesなど)を使う
 - Javaメソッドのラッパー(clojure.core/basesなど)を使う
 - bean 関数を使う
 - clojure.reflectを使う
 - hierarchy を利用する (clojure.core/parentなど)

などがあるようです。



### 直接の親クラス、インターフェースを取得する

```clojure
(type [])
;; => clojure.lang.PersistentVector

(:superclass (bean clojure.lang.PersistentVector))
;; => clojure.lang.APersistentVector

(.getSuperclass (type []))
;; => clojure.lang.APersistentVector

(vec (.getInterfaces (class [])))
;; => [clojure.lang.IObj clojure.lang.IEditableCollection]

(vec (:interfaces (bean clojure.lang.PersistentVector)))
;; => [clojure.lang.IObj clojure.lang.IEditableCollection]

(bases clojure.lang.PersistentVector)
;; => (clojure.lang.APersistentVector clojure.lang.IObj clojure.lang.IEditableCollection)

(:bases (r/reflect (type [])))
;; => #{clojure.lang.IEditableCollection clojure.lang.IObj clojure.lang.APersistentVector}

(parents (type []))
#{clojure.lang.APersistentVector clojure.lang.IObj clojure.lang.IEditableCollection}

```

### 親クラス、インターフェース一覧を取得する

```clojure
(supers clojure.lang.PersistentVector)
;; => #{clojure.lang.Indexed clojure.lang.IFn java.lang.Comparable
;;      java.lang.Object clojure.lang.IPersistentCollection clojure.lang.Sequential
;;      clojure.lang.ILookup clojure.lang.AFn clojure.lang.IMeta clojure.lang.Associative
;;      clojure.lang.Seqable clojure.lang.IHashEq java.io.Serializable clojure.lang.APersistentVector
;;      clojure.lang.IObj clojure.lang.IEditableCollection java.util.RandomAccess
;;      java.util.concurrent.Callable java.util.List clojure.lang.IPersistentStack
;;      java.util.Collection clojure.lang.Counted clojure.lang.Reversible java.lang.Iterable
;;      clojure.lang.IPersistentVector java.lang.Runnable}



(= (bases (type []))
   (supers (type [])))
;; => false

(every? (supers (type []))
        (bases (type [])))
;; => true


(= (supers (type []))
   (ancestors (type [])))
;; => true
```

-------------------------------------------------
## 関数の引数リストを取得する

`clojure.core/meta`を使います。

```clojure
(defn list-args [sym]
  (-> sym resolve meta :arglists))

(list-args 'list)
;; => ([& items])
(list-args 'deref)
;; => ([ref] [ref timeout-ms timeout-val])
```


-------------------------------------------------
## ネームスペース中でpublicな関数を取得する

`clojure.core/ns-publics`を使えばpublicなシンボルを取得できます。


```clojure
(use '[clojure.test :only (function?)])

(defn public-functions [ns]
  (->> (ns-publics ns)
       keys
       (filter function?)))
```

-------------------------------------------------
リンク

 - [clojure.reflect][clojure.reflect]



[clojure.reflect]: <http://clojure.github.com/clojure/clojure.reflect-api.html>

