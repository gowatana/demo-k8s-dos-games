# demo-k8s-dos-games

# 参考

* https://williamlam.com/2021/02/retro-dos-games-on-kubernetes.html
* https://github.com/paolomainardi/additronk8s-retrogames-kubernetes-controller
* https://dosgames.com/game/f-tetris/
* https://dosgames.com/game/super-street-fighter-ii-turbo/

# 1. コントローラーの準備

ファイルのダウンロード

```
$ git clone https://github.com/paolomainardi/additronk8s-retrogames-kubernetes-controller.git
$ cd additronk8s-retrogames-kubernetes-controller/
```

Namespace の作成

```
$ kubectl create ns games
namespace/games created
```

マニフェストの Apply

```
$ kubectl -n games apply -f k8s/manifests/
customresourcedefinition.apiextensions.k8s.io/games.retro.sparkfabrik.com created
clusterrolebinding.rbac.authorization.k8s.io/role-game-controller-binding created
clusterrole.rbac.authorization.k8s.io/role-game-controller created
serviceaccount/game-controller-service-account created
deployment.apps/game-controller created
Warning: resource namespaces/games is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
namespace/games configured
```

# 2. ゲームのの起動

マニフェストをダウンロード

```
$ cd
$ git clone https://github.com/gowatana/demo-k8s-dos-games.git
$ cd demo-k8s-dos-games/
```

Namespace の作成

```
$ kubectl create ns demo-01
namespace/demo-01 created
```

Tetris

```
$ kubectl -n demo-01 apply -f tetris.yml
```

LoadBalancer の EXTERNAL-IP にアクセスして、F-TETRIS.EXE と入力して Enter キー。

```
$ kubectl -n demo-01 get svc tetris-svc
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                       AGE
tetris-svc   LoadBalancer   100.64.207.15   192.168.61.205   80:32157/TCP,8081:31950/TCP   37m
```

SSF2 Turbo

```
$ kubectl -n demo-01 apply -f ssf2t.yml
```

LoadBalancer の EXTERNAL-IP にアクセスして、SSF2T.BAT と入力して Enter キー。

```
$ kubectl -n demo-01 get svc ssf2t-svc
NAME        TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                       AGE
ssf2t-svc   LoadBalancer   100.65.32.153   192.168.61.206   80:31653/TCP,8081:31925/TCP   35m
```

EOF
