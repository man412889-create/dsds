-- 1. 用戶表 (註冊後自動出現)
create table profiles (
  id uuid references auth.users on delete cascade primary key,
  full_name text,
  phone_text text,
  is_online boolean default false,
  is_busy boolean default false
);

-- 2. 小隊表 (記錄組隊狀態)
create table teams (
  id uuid default uuid_generate_v4() primary key,
  leader_id uuid references profiles(id),
  partner_id uuid references profiles(id),
  current_turn_index int default 0, -- 0 是隊長, 1 是隊員
  active boolean default true
);

-- 3. 名單表 (收集到的資訊)
create table leads (
  id uuid default uuid_generate_v4() primary key,
  team_id uuid references teams(id),
  customer_name text,
  customer_phone text,
  assigned_to uuid references profiles(id)
);
