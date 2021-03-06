# STEP-CodeReview

## 全体的な感想

経路を記録する方法として二つ考えられていて、それぞれのメリット、デメリットまで考察できていてすごいと思いました。確かに一方向リストに一つ前のノードを記録しておいて、最後のtargetが見つかったときに、どんどん遡っていけば経路がわかるし、dfsでもbfsでもどちらでも使えるのでモジュール化したりしてコードを簡潔にすることもできますね。私の方法と似ていますが、私はリストを使う方法が思いつかなかったのでむりやり文字列にして記録しました。リストにした方がきれいですね！

Tomokaさんは、タプルやリスト、一方向リストをうまく組み合わせて使えてすごいなと思いました。自分は入れ子みたいなのが苦手なのでTomokaさんを見習いたいです！

## homework_anotherSolution.py
    def dfs(links, pages, start_id, target_id):
    # start_id から　target_id への経路をdfsで調べる関数
    # - 存在していたら 経路（リスト型ret で、ret[i] =（深さ, ノード））を返す
    # - 存在しなかったら False を返す
    # 引数links のデータ構造（辞書型） key: id (from), value: id (to)
    container = collections.deque()  # `container` : list 型で 要素に tuple 型の (node_id, depth) を持つ
    depth = 0
    container.append((start_id, depth))
    visitedSet = set() # `visitedSet` : set型. ループしないために、一度調べたidを記録する.
    path = {} # `path`: 経路を記録する辞書型. key = depth, value = node_id

最初にそのモジュールがどんなもので、何を返すのか、引数はどんなものなのか、新しく定義された変数が何を表すもので、どんな型なのかが書かれていて読みやすいです！listではなくsetにすると探索の計算量がO(1)になるのは初めて知りました。勉強になります。

    while len(container) != 0:
        node_depth = container.pop() # stackの処理を行う
        path[node_depth[1]] = node_depth[0]
        
幅優先探索では深さが同じのノードをどんどん探すのでこの方法は使えないが、深さ優先探索ではどんどん深いノードを探すので、pathが変に書き換えられてしまうこともなく、一番深いノードまで探して見つからなくて戻った場合は、自動的にその余分な部分だけpathが書き換えられるので、とても良い方法だなと思いました。

    # 隣接しているnodeをcontainerに加える。
    if not (node_depth[0] in visitedSet):
      visitedSet.add(node_depth[0])
      if node_depth[0] in links:
        for follow in links[node_depth[0]]:
          if not (follow in visitedSet):
            container.append((follow, node_depth[1] + 1))
        visitedSet.add(node_depth[0])

    depth += 1
    
私はcontainerに入れる段階でvisitedにも入れるようにしました。最初のif文が省略できて少し計算量が減らせますが、このようにpopして判定を行った後にvisitedに入れることによって空間計算量を減らすことができますね！勘違いかもしれませんが、最後の二行はなくても大丈夫じゃないでしょうか？

## homework_TomokaSawada.py

コードがanothersolutionと似ているので細かいところは省略します。ともかさん自身もおっしゃっていたように、dfsとbfsでほとんど同じ作業をしているので、モジュール化した方が見やすくなると思います。
