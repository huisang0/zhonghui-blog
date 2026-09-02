---
title: 多场景下无人机烟幕干扰弹投放决策模型的构建与求解
published: 2026-08-31
description: ''
image: ''
tags: [数学建模]
category: '学术'
draft: false 
lang: ''
---

# 摘要


<a href="/public/多场景下无人机烟幕干扰弹投放决策模型的构建与求解.pdf" download="多场景下无人机烟幕干扰弹投放决策模型的构建与求解.pdf">点击下载原文</a>

针对空地导弹威胁下圆柱形真目标的烟幕遮蔽防护问题，本文构建了无人机-烟幕弹-导弹的协同建模与优化体系，旨在通过设计无人机飞行及烟幕弹投放策略，最大化真目标的有效遮蔽时长。

**针对问题一，** 通过建立导弹、无人机与烟幕弹的运动学模型和几何遮蔽判定模型，构建描述进入与离开遮蔽时刻的非线性方程组，采用**Newton-Raphson方法**求解方程组和**Halley方法**进行数值求解，得到有效遮蔽时长为1.44秒。

**针对问题二，** 采用**多阶段优化算法**求解无人机的最优烟幕弹投放策略。通过构建遮蔽效果模型，分析烟幕与导弹视线的动态关系，精确计算有效遮蔽时长。优化在航向、速度、投放时间和引信延时四个维度上进行，最终得到最优方案：无人机以70 m/s速度、177°航向飞行，起飞1秒投放，引信延时3秒。该方案实现遮蔽4.724秒。

**针对问题三，** 基于导弹-目标-无人机-烟幕的一体化模型，采用动态云团模拟和可见性判断，生成遮蔽状态。通过枚举起爆方案并结合优化算法进行搜索调优，再经**蒙特卡洛**扰动测试评估鲁棒性。得到最终方案：多弹遮蔽总时长达6.660秒。

**针对问题四，** 本文研究了使用三架无人机协同干扰导弹M1的烟幕投放策略优化问题。该问题属于**高维非线性优化**范畴，通过建立几何约束和时序协调模型，采用混合智能算法进行求解。算法融合了多种优化技术，通过并行搜索和局部精细化策略确保了解的质量和稳定性。最终获得最优遮蔽时长为13.19秒。

**针对问题五，** 采用基于**遗传算法**的协同优化策略，对多架无人机集群的航向、速度及多枚烟幕弹投放参数（时序与起爆高度）进行联合求解，有效克服了时空约束与弹目交会条件的复杂性。得到最终三枚导弹总共被有效遮蔽10.650 s。

**关键词：** 遮蔽时长优化；多阶段优化算法；混合智能算法；遗传算法；蒙特卡洛测试；投放策略优化

# 1 问题重述

## 1.1 问题背景

在现代的对抗场景中，烟幕干扰弹凭借其成本低、效果佳的优势，成为干扰敌方导弹、保护己方目标的重要手段。其核心原理为通过化学燃烧或爆炸形成烟幕或气溶胶云团，在己方目标前方的特定空域形成遮蔽，阻挡敌方导弹对目标的探测以及锁定。随着技术的发展，无人机已经成为烟幕干扰弹的重要搭载平台——具备长续航的无人机可在特定空域巡飞，受领任务后能够精准控制干扰弹的投放及起爆，实现对己方目标的高效保护。

在实际应用场景中，为使得烟幕干扰效果更为有效，需要合理设计无人机投放干扰弹的策略，如无人机飞行方向、速度，干扰弹投放点位置、起爆点位置等。本研究即围绕着不同场景下（无人机的数量、无人机投放干扰弹的数量、地方导弹的数量）无人机投放干扰弹的最优策略而展开。

## 1.2 问题描述

本任务聚焦"无人机投放烟幕干扰弹保护真目标"这一核心场景。以假目标为诱饵，敌方3枚空地导弹直奔假目标，需要通过5架预设初始位置的无人机FY1-FY5投放烟幕干扰弹，在敌方导弹与真目标之间形成遮蔽。

**问题一：** 仅使用FY1，保持以120 m/s的速度朝假目标方向等高以直线飞行。自受令1.5 s后投放1枚烟幕干扰弹，并设定3.6 s后起爆。求在此条件下，该弹对导弹M1的有效遮蔽时长。

**问题二：** 仍只使用FY1与1枚烟幕干扰弹，通过选择FY1的航向、巡航速度和烟幕干扰弹的投放点、起爆点等参数，最大化对M1的有效遮蔽时长，并给出对应的最优参数与策略。

**问题三：** 动用FY1与3枚烟幕干扰弹对M1实施干扰，设计三枚干扰弹的投放与起爆时空方案，并将结果整理到文件result1.xlsx。

**问题四：** 动用FY1、FY2、FY3三架无人机，各投放1枚烟幕干扰弹对M1进行干扰，设计三机的航向与速度及各弹的投放与起爆方案，以可能延长有效遮蔽时间，并将结果整理到文件result2.xlsx。

**问题五：** 出动5架无人机，每架至多投放3枚烟幕干扰弹，同时干扰M1、M2、M3等3枚来袭导弹，综合分配载弹与投放时空策略，尽可能增加对目标的有效遮蔽时长，并将结果整理到文件result3.xlsx。

# 2 问题分析

## 2.1 问题一的分析

问题一已知无人机FY1的航向和巡航速度，以及1枚烟幕干扰弹的投放点和爆炸点，要求得到干扰弹的遮蔽时长，这属于**多主体运动学与几何分析相结合的时空碰撞判定问题**。研究这一问题可以为后续多机、多弹协同设计提供基准，验证特定参数组合下的策略是否合理。解决问题一的流程如图1所示。


## 2.2 问题二的分析

问题二要求自定义无人机FY1的航向和速度，以及1枚烟幕干扰弹的投放点和爆炸点，通过合理的方案使得遮蔽时间尽可能长，这属于典型的**多参数非线性优化类数学问题**。解决问题二的流程如图2所示。


## 2.3 问题三的分析

问题三要求自定义无人机FY1的航向和速度，以及3枚烟幕干扰弹的投放点和爆炸点，通过合理的方案使得遮蔽时间尽可能长，这属于典型的**约束优化问题**。解决问题三的流程如图3所示。


## 2.4 问题四的分析

问题四属于动态优化与轨迹规划的组合优化问题，通常采用分层优化或序列决策方法求解。数据显示三架无人机空间分布分散，高度差异明显，而导弹M1轨迹确定性强，为预测提供基础。问题要求最大化遮蔽时间，需在飞行路径、投放时机和起爆高度间权衡。解决问题四的流程如图4所示。


## 2.5 问题五的分析

问题五要求用5架无人机对3枚来袭导弹进行干扰，核心是通过优化无人机飞行参数及烟幕弹投放、起爆点，最大化真目标有效遮蔽总时长，属于**多约束多目标的全局优化问题**。解决问题五的流程如图5所示。


# 3 模型假设与符号说明

## 3.1 模型假设

1.无人机投放干扰弹前瞬时调整方向，投放后沿水平直线匀速飞行（高度不变）。

2.干扰弹脱离无人机后，运动分为两段：

（1）投放后至起爆前：干扰弹水平匀速（与无人机同速）＋竖直自由落体；

（2）起爆后：烟幕云团为水平位置固定＋竖直匀速下沉。

3.导弹沿直线匀速飞行，方向始终指向假目标。

4.有效遮蔽判定标准：导弹视线（即导弹位置与真目标位置之间连线的连线）与烟幕云团存在交集，即导弹视线到烟幕球心的最小距离≤10 m。

5.在计算烟幕云团与真目标的空间关系时，将真目标看作质点，位置与真目标几何中心的位置作相同。

## 3.2 符号说明

| **符号**                 | **含义**                   |
| ------------------------ | -------------------------- |
| $\textit{\textbf{O}}$    | 假目标（坐标原点）位置向量 |
| $\textit{\textbf{T}}$    | 真目标位置向量             |
| $\textit{\textbf{M}}_0$  | 导弹初始位置向量           |
| $\textit{\textbf{M}}(t)$ | 导弹在时刻$t$的位置向量    |
| $\textit{\textbf{U}}_0$  | 无人机初始位置向量         |
| $\textit{\textbf{U}}(t)$ | 无人机在时刻$t$的位置向量  |
| $g$                      | 重力加速度                 |
| $s^*$                    | 视线上最近点参数           |
| $\text{tol}$             | 数值方法收敛容差           |
| $\text{max\_iter}$       | 最大迭代次数               |

# 4 模型建立与求解

## 4.1 问题一模型的建立与求解

### 4.1.1 模型的建立

**1.运动学及几何判定模型的建立**

（1）坐标系以及核心实体参数定义

以假目标为坐标原点$O$建立三维直角坐标系，水平面为$xOy$平面，竖直向上为*z*轴正方向。则真目标的位置（圆柱体的几何中心）为$T$，无人机FY1的初始位置为$\text{FY}1_0$，导弹
M1 的初始位置为$\text{M1}_0$。

（2）关键物理与时序参数定义

在模型的建立过程中，关键物理与时序参数的定义如表所示。

# 附录A   支撑材料文件清单

**序号**       **文件名**              **说明**

---

```
1             Q_1.m          解决问题一的MATLAB代码
  2             Q_2.m          解决问题二的MATLAB代码
  2             Q_3.m          解决问题三的MATLAB代码
  4             Q_4.m          解决问题四的MATLAB代码
  5             Q_5.m          解决问题五的MATLAB代码
  6          result1.xlsx       问题三的结果输出文件
  7          result2.xlsx       问题四的结果输出文件
  8          result3.xlsx       问题五的结果输出文件
  9       AI工具使用详情.pdf       AI工具使用详情
```

: 支撑材料文件清单

# 附录B    解决问题一的核心MATLAB代码

```
%% 运动学建模
Cf = @(t) C0 + [0, 0, -vs * (t - t0)];
Mf = @(t) M10 + VM * t * uh;
Mp = VM * uh;  
Cp = [0, 0, -vs];  
%% 视线遮蔽判定函数
cmd = @(t) cmp(t, Mf, Cf, T);
ts = linspace(t0, t0 + DT, 1000);
mv = inf;
ti = t0;
si = 0.5;

for i = 1:length(ts)
    t = ts(i);
    Mt = Mf(t);
    Ct = Cf(t);
    U = Ct - Mt;
    V = T - Mt;
    Vn = dot(V, V);
    ss = dot(U, V) / Vn;
    if ss > 0 && ss < 1
        d2 = dot(U, U) - (dot(U, V))^2 / Vn;
        val = abs(d2 - R^2);
        if val < mv
            mv = val;
            ti = t;
            si = ss;
        end
    end
end

[tin, sin] = n2s(ti, si, Mf, Cf, T, R, Mp, Cp);
gf = @(t) norm(Mf(t) - Cf(t))^2 - R^2;
gp = @(t) 2 * dot(Mf(t) - Cf(t), Mp - Cp);
gpp = 2 * norm(Mp - Cp)^2;
ts2 = linspace(tin, t0 + DT, 1000);
for i = 2:length(ts2)
    if gf(ts2(i-1)) < 0 && gf(ts2(i)) > 0
        to = (ts2(i-1) + ts2(i)) / 2;
        break;
    end
end
tout = hs(to, gf, gp, gpp);
Ts = tout - tin;

function [dm, ss] = cmp(t, Mf, Cf, T)
    Mt = Mf(t);
    Ct = Cf(t);
    U = Ct - Mt;
    V = T - Mt;
    Vn = dot(V, V);
    ss = dot(U, V) / Vn;
    if ss <= 0
        dm = norm(Mt - Ct);
    elseif ss >= 1
        dm = norm(T - Ct);
    else
        dm = sqrt(dot(U, U) - (dot(U, V))^2 / Vn);
    end
end

function [t, s] = n2s(t0, s0, Mf, Cf, T, R, Mp, Cp)
    t = t0;
    s = s0;
    mi = 100;
    tl = 1e-12;
    for it = 1:mi
        Mt = Mf(t);
        Ct = Cf(t);
        V = T - Mt;
        Vp = -Mp;
        W = Mt + s * V;
        Rv = W - Ct;
        F1 = dot(Rv, Rv) - R^2;
        F2 = dot(Rv, V);
        if abs(F1) < tl && abs(F2) < tl
            break;
        end
        dWdt = (1 - s) * Mp;
        dRdt = dWdt - Cp;
        J11 = 2 * dot(Rv, dRdt);
        J12 = 2 * dot(Rv, V);
        J21 = dot(dRdt, V) + dot(Rv, Vp);
        J22 = dot(V, V);
        dJ = J11 * J22 - J12 * J21;
        if abs(dJ) < 1e-15
            break;  
        end
        dt = -(J22 * F1 - J12 * F2) / dJ;
        ds = -(-J21 * F1 + J11 * F2) / dJ;
        t = t + dt;
        s = s + ds;
        s = max(1e-10, min(s, 1 - 1e-10));
        if abs(dt) < tl && abs(ds) < tl
            break;
        end
    end
end

function t = hs(t0, gf, gpf, gpp)
    t = t0;
    mi = 100;
    tl = 1e-12;
  
    for it = 1:mi
        g = gf(t);
        gp = gpf(t);
  
        if abs(g) < tl
            break;
        end
  
        dn = 2 * gp^2 - g * gpp;
        if abs(dn) < 1e-15
            dt = -g / gp;
        else
            dt = -2 * g * gp / dn;
        end
  
        t = t + dt;
  
        if abs(dt) < tl
            break;
        end
    end
end
```

# 附录C   解决问题二的核心MATLAB代码

```
%计算遮蔽时间
function st = f(p, f1, m, u, v, t, r, s, g, mx)
    th = p(1);
    vu = p(2);
    tr = p(3);
    tf = p(4);
    %轨道
    d = [cos(th), sin(th), 0];
    pr = f1 + vu * tr * d;
    pb = pr + vu * tf * d - [0, 0, 0.5*g*tf^2];
    tb = tr + tf;
    dt = 0.002;
    ts = tb:dt:(tb + mx);
    te = -1;
    tx = -1;
    for i = 1:length(ts)
        ti = ts(i);
        mt = m + v * ti * u;
        ct = pb;
        ct(3) = ct(3) - s * (ti - tb);
        ui = ct - mt;
        vi = t - mt;
        vn = sum(vi.^2);
        if vn < 1e-6
            break;
        end
        si = (ui * vi') / vn;
        si = max(0, min(1, si));
        if si <= 0
            dm = norm(mt - ct);
        elseif si >= 1
            dm = norm(t - ct);
        else
            w = mt + si * vi;
            dm = norm(w - ct);
        end
        %是否遮蔽了
        if dm <= r + 1e-6
            if te < 0
                te = ti;
            end
            tx = ti;
        elseif te > 0
            break;
        end
    end
    if te > 0 && tx > te
        st = tx - te;
    else
        st = 0;
    end
end
```

# 附录D   解决问题三的核心MATLAB代码

```
% 判断点c是否在由点m到点t的线段上，并且点c与线段的距离是否小于等于r
function b = f1(m, t, c, r)
    ab = t - m;   
    ap = c - m;   
    l2 = dot(ab, ab);   
    if l2 < 1e-12   
        b = (norm(c - m) <= r);  
        return;
    end
    s = dot(ap, ab) / l2;  
    s = max(0, min(1, s)); 
    q = m + s * ab;   
    b = (norm(c - q) <= r); 
end

% 计算云团在给定时间a时的半径
function r = f2(p, a)
    if a <= 0
        r = p.r0;  
        return;
    end
    r = sqrt(max(0, p.r0^2 + 2 * p.dx * a)) + p.al * a;  
    r = min(p.rm, r); 
end

% 计算云团在给定时间a时的z轴位置（高度）
function z = f3(p, h, a)
    if a <= 0
        z = h;  
        return;
    end
    z = h - p.vt * (1 - exp(-a / p.ts)); 
end

% 评估个体适应度，基于遮蔽时间和其他约束
function ft = f4(id, ctx)
    id = sort(unique(max(1, min(numel(ctx.cd), id)), 'ascend')); 
    if numel(id) < 3
        ft = 1e9;  
        return;
    end
    mk = ctx.cd(id(1)).mk | ctx.cd(id(2)).mk | ctx.cd(id(3)).mk; 
    tt = sum(mk) * ctx.p.dt; 
    td = sort([ctx.cd(id).td]);  
    d1 = td(2) - td(1);  
    d2 = td(3) - td(2); 
    pi = h1(ctx.p.mi - d1, ctx.p.hd) + h1(ctx.p.mi - d2, ctx.p.hd); 
    pw = 0;  
    if tt < ctx.p.tl 
        pw = pw + h1(ctx.p.tl - tt, ctx.p.hd);  
    end
    if tt > ctx.p.th  
        pw = pw + h1(tt - ctx.p.th, ctx.p.hd);
    end
    ft = abs(tt - ctx.p.tg) + ctx.p.pi * pi + ctx.p.pw * pw;  
end

% 计算Huber平滑惩罚
function y = h1(x, d)
    if x <= 0
        y = 0;  
    else
        if x <= d
            y = 0.5 * (x / d)^2;  
        else
            y = x - d / 2;  
        end
    end
end
```

# 附录E   解决问题四的核心MATLAB代码

```
%% 核心问题设置
uav_init = [17800,0,1800; 12000,1400,1400; 6000,-3000,700]; % FY1,FY2,FY3
M1_init  = [20000,0,2000];
v_M1     = 300;
target   = [0,200,0];
t_end    = 66.67;
g        = 9.8;
R        = 10;
% 12 维：3 架 * (theta,v,t0,dt)
patternMin = [0,70,0,0];
patternMax = [2*pi,140,t_end,t_end];
VarMin = repmat(patternMin,1,3);
VarMax = repmat(patternMax,1,3);
nVar   = numel(VarMin);   % 12
%% 核心目标函数
function totalTime = EvaluateShieldingTime_v3(sol,uav_init,M1_init,v_M1,target,t_end)
    R = 10; g=9.8;
    th   = [sol(1),sol(5),sol(9)];
    v    = [sol(2),sol(6),sol(10)];
    t0   = [sol(3),sol(7),sol(11)];
    dt_k = [sol(4),sol(8),sol(12)];
    % 严格边界约束
    for k=1:3
        v(k)    = clamp(v(k), 70, 140);
        t0(k)   = clamp(t0(k), 0, t_end-20);
        dt_k(k) = clamp(dt_k(k), 0.5, 20);
        % 确保不超过结束时间
        if t0(k) + dt_k(k) + 20 > t_end
            excess = t0(k) + dt_k(k) + 20 - t_end;
            if dt_k(k) - excess >= 0.5
                dt_k(k) = dt_k(k) - excess;
            else
                dt_k(k) = 0.5;
                t0(k) = max(0, t_end - 20.5);
            end
        end
    end
  
    t1 = t0 + dt_k;
  
    % 计算爆点
    C0 = zeros(3,3);
    for k=1:3
        vx = v(k)*cos(th(k)); 
        vy = v(k)*sin(th(k));
        exp_x = uav_init(k,1) + vx * t1(k);
        exp_y = uav_init(k,2) + vy * t1(k);
        exp_z = uav_init(k,3) - 0.5 * g * (dt_k(k)^2);
        exp_z = max(0, exp_z); % 确保高度非负
        C0(k,:) = [exp_x, exp_y, exp_z];
    end
  
    % 精确覆盖判定
    dir = (target - M1_init) / norm(target - M1_init);
  
    function flag = cov_at(t)
        M1pos = M1_init + dir * v_M1 * t;
        vL = target - M1pos;
        a = dot(vL,vL);
  
        if a <= 1e-15
            flag = false; 
            return;
        end
  
        for kk=1:3
            if t >= t1(kk) && t <= t1(kk)+20
                dt_smoke = t - t1(kk);
                C = C0(kk,:) - [0, 0, 3*dt_smoke];
                % 射线与球相交测试
                L0 = M1pos - C;
                b = 2*dot(L0,vL);
                c = dot(L0,L0) - R^2;
                D = b^2 - 4*a*c;
    
                if D >= -1e-10  % 数值容差
                    if D < 0, D = 0; end
                    sqrtD = sqrt(D);
                    u1 = (-b + sqrtD)/(2*a);
                    u2 = (-b - sqrtD)/(2*a);
        
                    % 检查交点是否在线段上
                    eps = 1e-8;
                    if (u1 >= -eps && u1 <= 1+eps) || (u2 >= -eps && u2 <= 1+eps)
                        flag = true;
                        return;
                    end
        
                    % 检查线段是否完全在球内
                    if (u1 < -eps && u2 > 1+eps) || (u2 < -eps && u1 > 1+eps)
                        flag = true;
                        return;
                    end
                end
            end
        end
        flag = false;
    end
    % 多分辨率扫描
    dt_coarse = 0.2;
    dt_fine = 0.01;
    % 粗扫描
    times_coarse = 0:dt_coarse:t_end;
    covered_coarse = false(size(times_coarse));
    for i=1:numel(times_coarse)
        covered_coarse(i) = cov_at(times_coarse(i));
    end
    if ~any(covered_coarse)
        totalTime = 0;
        return;
    end
    % 识别潜在区间
    intervals = [];
    for i=1:numel(times_coarse)-1
        if covered_coarse(i) || covered_coarse(i+1)
            intervals = [intervals; times_coarse(i), times_coarse(i+1)]; %#ok<AGROW>
        end
    end
    % 合并重叠区间
    if ~isempty(intervals)
        intervals = merge_intervals(intervals);
    end
    % 精细扫描
    covered_segments = [];
    for int_idx = 1:size(intervals,1)
        t_start = intervals(int_idx,1);
        t_end_int = intervals(int_idx,2);
        times_fine = t_start:dt_fine:t_end_int;
        covered_fine = false(size(times_fine));
        for i=1:numel(times_fine)
            covered_fine(i) = cov_at(times_fine(i));
        end
        % 提取覆盖段
        in_segment = false;
        seg_start = 0;
        for i=1:numel(times_fine)
            if covered_fine(i) && ~in_segment
                seg_start = times_fine(i);
                in_segment = true;
            elseif ~covered_fine(i) && in_segment
                covered_segments = [covered_segments; seg_start, times_fine(i-1)]; %#ok<AGROW>
                in_segment = false;
            end
        end 
        if in_segment
            covered_segments = [covered_segments; seg_start, times_fine(end)]; %#ok<AGROW>
        end
    end
    % 合并并计算总时间
    if isempty(covered_segments)
        totalTime = 0;
    else
        covered_segments = merge_intervals(covered_segments);
        totalTime = sum(covered_segments(:,2) - covered_segments(:,1));
    end
    if totalTime < 0, totalTime = 0; end
    if totalTime > t_end, totalTime = t_end; end
end
function merged = merge_intervals(intervals)
    if isempty(intervals)
        merged = [];
        return;
    end
    [~,idx] = sort(intervals(:,1));
    intervals = intervals(idx,:);
    merged = intervals(1,:);
    for i=2:size(intervals,1)
        if intervals(i,1) <= merged(end,2) + 1e-6
            merged(end,2) = max(merged(end,2), intervals(i,2));
        else
            merged = [merged; intervals(i,:)]; %#ok<AGROW>
        end
    end
end
%% 核心优化算法 - PSO
function [best_sol, best_val] = core_pso(VarMin, VarMax, uav_init, M1_init, v_M1, target, t_end)
    nVar = numel(VarMin);
    pop_size = 50;
    max_iter = 100;
    % 初始化种群
    pop = zeros(pop_size, nVar);
    for i = 1:pop_size
        pop(i,:) = VarMin + rand(1,nVar).*(VarMax-VarMin);
    end
    % 初始化速度和个体最优
    vel = zeros(pop_size, nVar);
    p_best = pop;
    p_best_val = zeros(pop_size, 1);
    % 评估初始种群
    for i = 1:pop_size
        p_best_val(i) = EvaluateShieldingTime_v3(pop(i,:), uav_init, M1_init, v_M1, target, t_end);
    end
    % 全局最优
    [g_best_val, idx] = max(p_best_val);
    g_best = pop(idx,:);
    % PSO参数
    w_max = 0.9; w_min = 0.4;
    c1 = 2; c2 = 2;
    % 主循环
    for iter = 1:max_iter
        w = w_max - (w_max - w_min) * iter / max_iter;
        for i = 1:pop_size
            r1 = rand(1, nVar);
            r2 = rand(1, nVar);
            % 更新速度
            vel(i,:) = w * vel(i,:) + c1 * r1 .* (p_best(i,:) - pop(i,:)) + c2 * r2 .* (g_best - pop(i,:));
            % 速度限制
            vel_max = 0.1 * (VarMax - VarMin);
            vel(i,:) = max(min(vel(i,:), vel_max), -vel_max);
            % 更新位置
            pop(i,:) = pop(i,:) + vel(i,:);
            pop(i,:) = max(min(pop(i,:), VarMax), VarMin);
            % 评估适应度
            fitness = EvaluateShieldingTime_v3(pop(i,:), uav_init, M1_init, v_M1, target, t_end);  
            % 更新个体最优
            if fitness > p_best_val(i)
                p_best(i,:) = pop(i,:);
                p_best_val(i) = fitness;
            end  
            % 更新全局最优
            if fitness > g_best_val
                g_best = pop(i,:);
                g_best_val = fitness;
            end
        end
        if mod(iter, 20) == 0
            fprintf('Iter %d: Best = %.4f\n', iter, g_best_val);
        end
    end  
    best_sol = g_best;
    best_val = g_best_val;
end
%% 辅助函数
function y = clamp(x, lo, hi)
    y = max(lo, min(hi, x));
end
%% 主运行代码
[best_solution, best_fitness] = core_pso(VarMin, VarMax, uav_init, M1_init, v_M1, target, t_end);
fprintf('最优遮蔽时间: %.4f 秒\n', best_fitness);
fprintf('最优解:\n');
for i = 1:3
    idx = (i-1)*4;
    fprintf('UAV%d: theta=%.3f, v=%.1f, t0=%.1f, dt=%.1f\n', ...
        i, best_solution(idx+1), best_solution(idx+2), best_solution(idx+3), best_solution(idx+4));
end
```

# 附录F   解决问题五的核心MATLAB代码

```
function [neg,D]=fobj(x,U0,M0,T0,vm,vdir,g,Rr,Tr,vz,tg,nu,kmax,nm)
    i=0;
    v =x(i+1:i+nu); 
    i=i+nu;  
    ph=x(i+1:i+nu); 
    i=i+nu;   
    td=x(i+1:i+nu*kmax);
    i=i+nu*kmax; 
    ta=x(i+1:i+nu*kmax);     
    % ---------- 约束惩罚项 ----------
    pen=0;
    for u=1:nu
        s=(u-1)*kmax+(1:kmax);
        tdu=td(s); tau=ta(s);
        tds=sort(tdu);
        if any(diff(tds)<1), pen=pen+1e3*sum(max(0,1-diff(tds))); end
        tdet=tdu+tau;
        pen=pen+1e3*sum(max(0,tdet-tg(end)))+1e3*sum(max(0,-tdu));
    end

    % ---------- 计算烟雾弹轨迹 ----------
    du=[cos(ph(:)),sin(ph(:)),zeros(nu,1)];  
    B=[]; 
    for u=1:nu
        for k=1:kmax
            id=(u-1)*kmax+k;
            if td(id)<0 || ta(id)<=0, continue; end
            pr=U0(u,:)+v(u)*td(id)*du(u,:);
            pb=pr+v(u)*ta(id)*du(u,:)+[0,0,-0.5*g*ta(id)^2];
            tb=td(id)+ta(id);
            B=[B; pb,tb,tb+Tr]; % (x,y,z,t_burst,t_end)
        end
    end

    % ---------- 计算每枚导弹被遮蔽时间 ----------
    cv=zeros(nm,1);
    for m=1:nm
        m0=M0(m,:); vd=vdir(m,:);
        hit=norm(m0)/vm; 
        vis=false(size(tg));
        for it=1:numel(tg)
            t=tg(it); if t>hit, break; end
            mp=m0+vm*t*vd;
            ok=false;
            for b=1:size(B,1)
                tb=B(b,4); te=B(b,5);
                if t<tb||t>te, continue; end
                c=B(b,1:3)+[0,0,vz*(t-tb)]; 
                if dseg(c,mp,T0)<=Rr, ok=true; break; end 
            end
            vis(it)=ok;
        end
        cv(m)=sum(vis)*(tg(2)-tg(1)); 
    end

    % ---------- 目标函数 ----------
    tot=sum(cv);  
    neg=-(tot)+pen;  
    D.summary=table((1:nm)',cv,'VariableNames',{'Missile','Coverage_s'});
end

function d=dseg(c,a,b)
    ab=b-a; ac=c-a;
    t=dot(ac,ab)/(dot(ab,ab)+eps);
    t=max(0,min(1,t));
    p=a+t*ab;
    d=norm(c-p);
end
```
$$

